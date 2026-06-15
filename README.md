# -JupyterLab-vscode-ssh-

启动（关键）

只开一个：
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root

进入容器端口8888后浏览器输入token是
在其他终端获取token

✅ 3. Web 界面已进入

你现在看到：

Launcher 启动页面 ✔
Notebook 入口 ✔
Terminal ✔
Console ✔
Python kernel（ipykernel / lerobot）✔

👉 说明：JupyterLab UI 已完全加载成功

✅ 4. Kernel 环境正常

可用环境：

Python 3 (ipykernel)
Python (lerobot)

👉 说明：

conda 环境 ✔ 正常
MuJoCo / LeRobot 环境已可用
✅ 5. 系统状态稳定

当前状态：

无重复 Jupyter 进程 ✔
无端口冲突 ✔
token 唯一 ✔
VNC 不受影响 ✔


很好，现在已经进入“可复现实验成功状态”了，我帮你把**整套能跑起来的步骤做一个清晰总结**，以后你照这个流程就不会乱。

---

# 🚀 MuJoCo + LeRobot（VNC版）完整可运行流程总结

---

# 🧠 一、整体流程图（你现在已经跑通）

```text
1. SSH登录服务器
        ↓
2. 启动VNC桌面
        ↓
3. 进入conda环境 (lerobot)
        ↓
4. 启动Jupyter Lab
        ↓
5. 打开 1.collect_data.ipynb
        ↓
6. 启动MuJoCo环境
        ↓
7. VNC显示机械臂
        ↓
8. 键盘teleop控制
        ↓
9. 采集demo数据
```

---

# 🧱 二、一步一步标准操作（可复用）

---

## 🟢 Step 1：进入服务器

```bash
ssh user@server_ip
```

---

## 🟢 Step 2：进入环境

```bash
conda activate lerobot
cd /workspace/lerobot-mujoco-tutorial-master
```

---

## 🟢 Step 3：启动Jupyter（关键）

```bash
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root
```

---

## 🟢 Step 4：VNC打开浏览器

输入：

```text
http://localhost:8888
```

---

## 🟢 Step 5：打开 notebook

打开：

```text
1.collect_data.ipynb
```

---

## 🟢 Step 6：初始化MuJoCo环境

notebook里运行：

```python
env = SimpleEnv(xml_path=...)
```

---

## 🟢 Step 7：启动可视化

```python
env.init_viewer()
```

👉 VNC出现：

* robot
* mug
* plate

---

## 🟢 Step 8：开始控制循环（核心）

```python
while True:
    env.step_env()
    env.render(teleop=True)
```

---

## 🎮 Step 9：键盘控制（重点）

| 功能    | 按键      |
| ----- | ------- |
| XY移动  | W A S D |
| Z轴    | R F     |
| 旋转    | Q E     |
| 夹爪    | SPACE   |
| reset | Z       |

---

## 🟢 Step 10：开始采集数据

你做：

> 🤖 抓 mug → 放 plate

系统自动记录：

```text
demo_data/
├── observation.image
├── observation.state
├── action
```

---

# 📦 三、实验产物（非常重要）

每一条 episode 会生成：

```text
data/
 ├── chunk-000/
 │    ├── episode_000001.parquet
meta/
 ├── episodes.jsonl
 ├── stats.json
```

| 动作    | 按键      |
| ----- | ------- |
| 前后左右  | W A S D |
| 上下    | R / F   |
| 旋转    | Q / E   |
| 夹爪    | SPACE   |
| reset | Z       |



现在可以直接进入实验：

📓 打开 Notebook 跑 MuJoCo
🤖 做 LeRobot 训练
🖥 用 Terminal 跑脚本
🎮 VNC 里看仿真窗口


如果第二次采集完成，而且没有覆盖原来的数据，那么你现在应该至少有：

find demo_data -name "*.parquet"

类似：

demo_data/data/chunk-000/episode_000000.parquet
demo_data/data/chunk-000/episode_000001.parquet

建议马上确认一下：

find demo_data -name "*.parquet" | wc -l

如果输出：

2

说明两条 episode 都保存成功了。
每隔几条检查一次

查看数量：

find demo_data -name "*.parquet" | wc -l

查看具体文件：

find demo_data -name "*.parquet" | sort


可以，完全没问题，而且你现在这个状态**是可以安全停的**。

---

# 🔋 你现在可以放心做的事

## ✔ 直接关机 / 断电 / 关闭 terminal 都可以

因为你已经确认：

```text id="p1k2m7"
episode_000000.parquet
episode_000001.parquet
```

👉 数据已经落盘保存了（不是临时内存）

---

# 🧠 明天回来怎么继续（重点）

你只需要按这个流程：

---

## 🚀 Step 1：重新进入环境

```bash id="a8m2p1"
conda activate lerobot
cd /workspace/lerobot-mujoco-tutorial-master
```

---

## 🚀 Step 2：检查数据还在不在

```bash id="q7m1k8"
find demo_data -name "*.parquet"
```

如果看到：

```text id="c2n9k1"
episode_000000.parquet
episode_000001.parquet
```

说明数据完整保留。

---

## 🚀 Step 3：打开 Jupyter

```bash id="v1m8k3"
jupyter lab
```

进入：

```text id="n8k2p7"
1.collect_data.ipynb
```

---

## ⚠️ 明天最重要的一点（防止你又踩坑）

如果再次看到：

```text id="m3k8p1"
Do you want to delete it? (y/n)
```

👉 一定选：

```text id="k2m7q9"
n
```

---

## 🚀 Step 4：继续采集

你现在已经有：

```text id="p9m2k1"
episode_000000
episode_000001
```

明天会自动接着变成：

```text id="b7m1k8"
episode_000002
episode_000003
...
```

---

# 🧠 关键提醒（非常重要）

你现在这个系统已经“进入正轨”，所以：

### ❌ 不要再 rm -rf demo_data

### ❌ 不要重新初始化整个 dataset

### ❌ 不要 Run All notebook

---

# 💡 最佳实践（给你之后用）

以后每天采集就是：

```text id="z1m8k3"
1. 打开 jupyter
2. 运行 init cell
3. 运行 collect cell
4. 采 5~10 条
5. 关掉
```

---

# 🔋 总结一句话

你现在状态是：

> ✅ 数据已经安全保存
> ✅ 可以随时中断
> ✅ 明天可以无缝继续采集

---

如果你明天回来想升级一步（比如“一键采集 + 自动记录 + 不用管 notebook”），我可以帮你把它改成一个：

```bash id="x8m2k9"
python collect.py --episodes 10
```

那种完全不会乱的版本。
