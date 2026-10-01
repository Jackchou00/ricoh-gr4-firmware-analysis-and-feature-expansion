# Developer guide / 开发者入门

## 1. Choose a sample / 确定样本

Obtain firmware from an official source yourself. The published GR IV container analysis uses `fwdc248b.bin`, public version 1.11, internal bytes `01 0B 0A 07`, length 38,648,776 bytes, SHA-256:

```text
a2f664dfca034059eb0fd6e18ab08684c326b4a034d85c164dad7e1ec9b5655f
```

Record your own sample's hash before comparing addresses. A matching public version alone is not enough. Do not commit the firmware or extracted data.

自行取得固件并核对哈希。公开版本相同不代表样本完全相同，地址比较必须说明对应样本。不要提交固件或解包数据。

## 2. Inspect and decode / 检查与解码

```sh
python3 tools/inspect_firmware.py /path/to/fwdc248b.bin
python3 tools/inspect_firmware.py /path/to/fwdc248b.bin --unpack /tmp/gr4-decoded.bin
```

Run from the repository root. The tool reports header/version fields, checksum and decoded frame information; it does not produce an installable update. The additive checksum is not evidence that signature verification is absent.

在仓库根目录执行。该工具检查包头、版本、校验及解码帧，不生成可安装更新包。发现加法校验不能据此排除签名验证。

## 3. Map file offsets to runtime addresses / 地址映射

For the documented GR IV 1.11.10.7 payload, the RTOS segment starts at decoded-file offset `0x17D10` and loads at `0x53000000`:

```text
runtime address = 0x53000000 + (decoded file offset - 0x17D10)
decoded file offset = 0x17D10 + (runtime address - 0x53000000)
```

This mapping applies to that RTOS segment, not the whole payload or the original compressed container. Label every offset and address with its space. Confirm instruction mode and mapped bytes before interpreting disassembly. The embedded Linux device tree describes its own CPU and memory view; it does not describe the entire camera's processor configuration or physical RAM.

该公式仅用于上述 RTOS 段，不能套到整个载荷或压缩包。记录地址时注明地址空间，并核对指令模式与实际字节。Linux 设备树描述 Linux 可见范围，不能直接作为整机处理器配置或物理 RAM 总量。

See [system-layout findings](../firmware-and-shutdown-image-research.md#a7linux-内存映射) and [Debug tracing](../gr4-debug-mode-analysis.md) for worked examples.

## 4. Use device probes separately / 区分设备探测

[mtp_probe_readonly.py](../../tools/mtp_probe_readonly.py) requires PyUSB and queries the connected camera's device and storage information. [ic_probe_readonly.swift](../../tools/ic_probe_readonly.swift) uses macOS ImageCaptureCore. Neither provides resource upload. The TTL [model probe](../../examples/identify-gr4-model.ttl.example) reads a product header internally and writes a report to SD; it requires Script to be enabled and is a separate interface.

USB 工具的只读查询、机内 TTL 脚本和离线固件分析是不同接口。设备结果需记录机型、固件版本、连接模式及相关配置；不能从一次查询推断其他模式或版本均相同。

## 5. Reproduce and contribute / 复现与提交

For the full host test suite, use Python 3.10+ with Pillow:

```sh
python3 -m pip install Pillow
python3 -m unittest discover -s tests -v
git diff --check
```

These tests cover host tools and limited TTL control-flow models, not hardware, storage timing or the camera's JPEG decoder. Run checks appropriate to the changed tool.

Use the [research-note template](research-note-template.md) for new findings. Include sample identity, commands, addresses, evidence, observed/static/inferred status and unresolved questions. For device-writing experiments, describe the backup, readback and recovery procedure before documenting success.

电脑测试不验证相机硬件、存储时序或 JPEG 解码器。新增发现请使用[研究记录模板](research-note-template.md)，注明样本、命令、地址、证据类型及未解决问题。涉及写入时必须记录备份、读回及恢复方法。
