# idempotent-recovery

- 状态: `暂缓`
- 优先级: 后做
- 本块独立，不依赖其他 `tasks/`

崩溃后重跑，最终状态必须与只成功跑一次相同。当前模型对这类题偏熟，先不投入。

设计见 [PROPOSAL.md](PROPOSAL.md)。
