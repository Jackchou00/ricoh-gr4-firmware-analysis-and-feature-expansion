# Research note template / 研究记录模板

Copy this file to a descriptive name under `docs/research/`. Keep firmware, device dumps and private assets outside the repository.

复制到 `docs/research/` 并使用具体主题命名，不提交固件、机身转储或私人素材。

## Question / 研究问题

What behavior or interface is being investigated?

## Sample / 样本

- Camera model and firmware version:
- Sample length and SHA-256:
- Test date and relevant configuration:

## Method / 方法

Commands, tool versions and steps required to reproduce the finding.

## Addresses / 地址

| Value | Space: container offset / decoded offset / runtime address | Meaning |
| --- | --- | --- |
| | | |

## Evidence / 证据

Separate static analysis, host tests, device observations and inference. Include relevant bytes, call paths or sanitized output, with a reference to their source.

## Result and limits / 结果与限制

What was established? What remains unknown? Does this contradict an earlier finding?

## Device writes and recovery / 设备写入与恢复

If applicable: original state, backup, write scope, complete readback checks, recovery method and whether recovery was actually tested. Otherwise state that the experiment does not write to the device.

## Next steps / 后续问题

What evidence would confirm or reject the remaining hypothesis?
