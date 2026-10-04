# Feature expansion / 功能扩展

Feature extensions build on the firmware interfaces documented in this repository. For firmware architecture and interface research, start with the [research index](../research/README.md).

功能扩展基于本仓库记录的固件接口开展。固件结构和接口分析见[研究索引](../research/README.md)。

## Custom shutdown images / 自定义关机画面

Replace the camera's shutdown image through the TTL resource interface, with backup and restoration. Choose the guide for your model.

通过 TTL 资源接口替换关机图片，支持原图备份和恢复。请按机型选择指南。

| Camera / 机型 | Guide / 指南 | Evidence / 验证情况 |
| --- | --- | --- |
| GR IV, HDF, Monochrome | [English](../gr4-family-shutdown-workflow.md) · [中文](../gr4-family-shutdown-workflow.zh-CN.md) | Identification and target paths have three-body evidence; combined workflows are offline-tested / 识别和目标路径有三机型依据，组合流程仅离线测试 |
| GR IIIx Urban Edition 1.60 | [Report and procedure / 报告与流程](../gr3x-urban-160-shutdown-image.md) | Replacement and restoration verified on one body; new wrappers are not fully camera-qualified / 单机替换及恢复已验证，新包装未整套实测 |
| GR IIIx HDF 1.60 | [Report and examples / 报告与示例](../gr3x-hdf-160-shutdown-image.md) | Replacement and final readback verified on one body; restore wrapper untested / 单机替换与最终读回已验证，恢复包装未实测 |
| Standard GR IV, original templates / 普通 GR IV 旧模板 | [Original report / 原始报告](../firmware-and-shutdown-image-research.md), [templates / 模板](../../examples/README.md) | Original copy workflow tested on one body; later guards have offline tests / 单机复制流程已验证，后加保护仅离线测试 |

### Entry and preparation / 入口与准备

Generate files on a computer, then follow the selected guide for copying them and enabling Script:

```sh
python3 tools/create_factory_entry.py ./entry
python3 tools/create_factory_entry.py ./entry-urban --model gr3x-urban-160
python3 tools/create_factory_entry.py ./entry-hdf --model gr3x-hdf-160
```

For the GR IV-family guide, copy `00078560.636` and `DEVELOP.MOD` to the SD-card root. With the camera off, hold MENU while powering on to enter the factory menu. Enable only Script, then shut down before removing the card. The family workflow uses FAT32; do not treat this entry method as universal firmware support. Urban and GR IIIx HDF entries are documented separately.

GR IV 系列入口为卡根目录的 `00078560.636` 与 `DEVELOP.MOD`，关机时按住 MENU 开机进入工厂菜单，仅开启 Script，再关机取卡。系列流程使用 FAT32。Urban 和 GR IIIx HDF 使用各自记录的入口及传输方法，请按独立指南操作。

本次 GR IIIx HDF 1.60 实机使用 `gr3x-hdf-160` 生成的入口文件；该入口只在一台 HDF 上验证。相同文件名和字节也用于 Urban 1.60，不表示后续资源路径或写入流程通用。

### Backup and verification / 备份与校验

Keep the original and a second copy on your computer for each body. Verify complete SHA-256 readbacks and the visible screen. Empty readbacks indicate failure or incomplete execution. Archive failed attempts before investigating; do not retry blindly. Remove the startup script and disable Script after completion.

每台机身单独保存原图及电脑副本，以完整 SHA-256 和实际画面核对结果。空读回不是成功，失败时保留现场并排查，结束后移除启动脚本并关闭 Script。已经覆盖且未备份的原图无法由这些工具找回。

## File-operation checks / 文件操作检查

Before writing internal resources, test a small SD-to-SD copy and verify its complete contents on the computer. See the [TTL file-operation findings](../research/gr4-ttl-filecopy-filesystems.md) for the reported failure and its limits. This preflight recommendation has not been added to or validated as part of the existing templates.

写入前先用小文件核对卡内复制，再在电脑上校验完整内容。该预检建议尚未集成进现有模板，不能当作已完成的保护功能。
