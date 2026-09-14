# transition-replay-controller

- 状态: `方案`
- 来源: HyperARC ARC-AGI-3 策略图 + PlanVerifier
- 本块独立

先探索建转移图，再重放最短已证路径。重放分叉必须中止。非法、致死、已结束态上的动作不得交给环境。
