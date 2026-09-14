# verified-candidate-merge

- 状态: `方案`
- 来源: HyperARC ARC-AGI-2 提交合并
- 本块独立

两槽提交合并：锁死第一槽，第二槽只收「训练全对、形状合法、且与第一槽不同」的候选。超时必须吐出合法 fallback，不能让整份提交缺行。
