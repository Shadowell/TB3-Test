# HyperARC 能抽出什么 TB3 题

来源：`/Users/jie.feng/Dev/Github/Private/HyperARC`  
结论：**不要搬 ARC 原题。** 能用的是「模型提议、控制器校验」这一层工程。

HyperARC 自己的原则正好对上 Klavis / TB3：

> models propose, HyperARC verifies.

## 项目在干什么

打 ARC Prize 2026，两条赛道：

- **ARC-AGI-3**：交互游戏。探索建转移图，再最短路径重放。
- **ARC-AGI-2**：程序合成。符号候选 + LLM 程序，全部要过训练对才准入。

难的不是「再猜一个网格变换」，而是：合法性、转移校验、生命周期、预算、提交格式不能崩。

## 不要抽成 TB3 的

| 不要用 | 原因 |
|---|---|
| 官方 ARC-AGI-1/2 谜题 | 训练语料里大量存在，模型可能做对；也不是「付钱的工程活」 |
| Kaggle notebook / 提交包 | 环境太重，依赖比赛网关 |
| LoRA / vLLM / GPU 微调 | TB3 资源窄，难在基建不在任务设计 |
| `ts01`–`ts200` 整游戏当考题 | 规则一旦公开，像解谜不像工程；也难防搜 |
| 直接贴 HyperARC 源码当 oracle | 题目必须原创，私有数据，不能交仓库副本 |

## 值得抽的（已建成独立任务块）

按推荐顺序：

1. [`verified-candidate-merge`](../tasks/verified-candidate-merge/)  
   来自 `merge_guard` + `shape_guard` + `chirality_filter` + `runtime_guard`  
   两槽提交：锁死 top-1，只有过验证且足够多样的才能进 top-2；超时必须给出合法 fallback。

2. [`transition-replay-controller`](../tasks/transition-replay-controller/)  
   来自 `HyperARCPolicy` + `PlanVerifier`  
   先探索建图，再重放最短已证路径；重放一旦分叉必须中止；非法动作进不了环境。

3. [`sandboxed-program-accept`](../tasks/sandboxed-program-accept/)  
   来自 `repl.py` + `qwen_induction` 截断恢复  
   从残缺代码里抢救出能跑的程序，沙箱执行，训练样例 100% 命中才接受。

4. [`collision-adaptive-planner`](../tasks/collision-adaptive-planner/)  
   来自世界模型：撞墙在线学习、自适应墙阈值、钥匙-门、可推物块  
   规划必须用学到的物理，不能靠盲走。

这四块都还没实现。和原来的 `replay-and-reconcile` 不冲突：那是账本；这些是竞赛控制器。可以并行写，评测过了再选一道交。

## 和 Klavis 的匹配

HyperARC 日常就是在做 Klavis 卖的那种数据：可验证轨迹、隐藏测试、gold path、Docker 化环境。用控制器题而不是 ARC 谜题，面试时也好讲：你懂「提议 vs 校验」这条边界。
