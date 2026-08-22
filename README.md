# codex-edit-fusion

> 判①产物：codex 的容错定位算法 + dsh 的原子提交保障，合成一个编辑器。

## 吸收来源
- apply-patch (5,441) 仅取 parser/streaming_parser/seek_sequence/file_update 算法
- 嫁接目标：dsh packages/fs/fs-local fsio.ts 原子写层 + replaceIfVersion CAS

## 功能边界
**做**：四级渐进匹配（精确→去尾空白→trim→Unicode 归一）；流式补丁应用；EOF 锚定；原子写入与失败回滚。

**不做**：不做 git 操作；不实现编辑 UI。

## API 草图
```
applyPatch(patchText, fs): AppliedPatchDelta
seekSequence(lines, pattern, start, eof): number|None
```

## 验收标准
原版 seek_sequence.rs 193 行测试全数翻译通过；模糊补丁应用率不低于上游；写失败零损坏。

## 上游同步
基于 openai/codex@970b7f2ff4f6（Apache-2.0）。季度 diff 由 dsh-codex-ledger CI 触发，见 ledger/coverage.yaml 对应行。
