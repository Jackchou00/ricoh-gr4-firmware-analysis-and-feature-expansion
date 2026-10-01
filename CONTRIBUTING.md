<!-- License: see LICENSE (earlier Apache-2.0 grants remain in force) -->

# Contributing

Contributions to firmware analysis and feature experiments are welcome, including format descriptions, address maps, read-only probes, reproducible findings and tests.

## Where changes belong

| Content | Location |
| --- | --- |
| Firmware and interface findings | `docs/research/` |
| Feature extension guides and experiments | `docs/extensions/` |
| Analysis and generation utilities | `tools/` |
| Minimal device examples | `examples/` |
| Host-side checks | `tests/` |

Existing reports and script paths remain valid. Link new findings from the relevant index. Use the [research-note template](docs/research/research-note-template.md) rather than burying technical evidence in the project homepage.

## Evidence and review

- Identify the camera, firmware version and sample SHA-256. Label container offsets, decoded offsets and runtime addresses separately.
- Distinguish static analysis, host tests, device observations and inference. State what was not verified, including cross-version compatibility.
- Include reproducible commands, relevant call paths or sanitized output. Record failed experiments and corrections when they affect the conclusion.
- For a feature experiment, identify the interface or code path being changed and explain how existing behavior is preserved.
- For device writes, include the original-state backup, exact write scope, complete readback checks and recovery procedure. Say whether recovery was actually tested.
- Run checks appropriate to the change. Full host tests require Python 3.10+ and Pillow: `python3 -m unittest discover -s tests -v`. They do not validate camera hardware.
- Do not submit firmware, extracted system images, brand artwork, camera readbacks, serial numbers or private user data.

## License

New contributions must be material you own or have authority to submit under the GR IV Project Noncommercial Source License 1.0. By submitting for inclusion, you agree that contributions may be distributed under the current license; you retain copyright. Separate commercial licensing requires permission from the relevant copyright holders.

Earlier contributions and revisions released under Apache License 2.0 keep their original grants. See [LICENSE](LICENSE), [NOTICE](NOTICE) and [README](README.md#license). There is no warranty or support commitment.

## 中文

欢迎固件格式、地址映射、只读探针、可复现发现及功能实验。研究记录放入 `docs/research/`，功能扩展指南放入 `docs/extensions/`，工具、示例和测试保留各自目录。

提交时注明机型、固件版本、样本哈希和地址空间，区分静态分析、电脑测试、实机观察与推测。提供复现命令和验证限制；涉及写入时，附原始状态备份、写入范围、完整读回及恢复方案，并注明是否实测恢复。不要提交固件、机身转储、序列号、私人数据或无权分发的图稿。授权要求以上方说明及 LICENSE 为准。
