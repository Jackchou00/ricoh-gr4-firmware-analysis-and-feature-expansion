<!-- License: see LICENSE (earlier Apache-2.0 grants remain in force) -->

# Ricoh GR IV Firmware Analysis and Feature Expansion

[English](#english) · [简体中文](#中文)

## English

Firmware research and shutdown-image tools for Ricoh GR cameras. Includes firmware-container analysis, read-only USB/MTP probes, factory-menu notes, and scripts to back up, replace and restore shutdown images.

Image replacement runs through an SD-card TTL script and does not require flashing firmware. Firmware, extracted resources and artwork are not included.

### Camera workflows

| Camera | Guide | Status |
| --- | --- | --- |
| GR IV | [GR IV family](docs/gr4-family-shutdown-workflow.md) | Original replacement tested on one body; identification tested on 1.11 |
| GR IV HDF | [GR IV family](docs/gr4-family-shutdown-workflow.md) | Identification and target path tested on 1.11 |
| GR IV Monochrome | [GR IV family](docs/gr4-family-shutdown-workflow.md) | Identification and target path tested; exact firmware version not recorded |
| GR IIIx Urban Edition 1.60 | [Urban workflow](docs/gr3x-urban-160-shutdown-image.md) | Replacement, restoration and full readback verified on one body |

The new combined GR IV-family workflows and Urban backup/restore wrappers have offline tests, but have not been tested end to end on cameras. Other models and firmware versions are unverified.

### Getting started

Read your camera's guide before running any script. Generate factory-menu entry files on your computer:

```sh
# GR IV family
python3 tools/create_factory_entry.py ./entry

# GR IIIx Urban Edition 1.60
python3 tools/create_factory_entry.py ./entry-urban --model gr3x-urban-160
```

The GR IV-family guide covers automatic model selection, backup, replacement and restoration. It uses a FAT32 card and a 720×480 JPEG matching the original file's byte length. Urban uses a separate two-stage installation method; do not use GR IV templates on it.

Always save the original image on your computer before replacing it. Keep backups separate for each camera, compare complete SHA-256 readbacks, and check the actual shutdown screen. An empty readback is a failed or incomplete attempt. Remove `script/startup.ttl` and disable Script when finished.

The original standard GR IV `*-goodbye.ttl.example` templates still use `GBBACK.JPG`. Existing users can keep that workflow; do not mix its backup names with family templates or use it on HDF/Monochrome. A lost original cannot be recovered by backing up an already modified image.

### Tools and documentation

- [Firmware and shutdown-image research](docs/firmware-and-shutdown-image-research.md)
- [Debug mode and factory-menu analysis](docs/gr4-debug-mode-analysis.md)
- [GR IV-family model identification](docs/gr4-model-identification.md)
- [Research log](docs/research-log.md)
- [tools/](tools/): firmware inspection, USB/MTP probes, JPEG preparation and script generators
- [examples/](examples/): TTL templates

Copying TTL templates needs no Python. Entry generation and basic analysis use Python 3. GR IV-family Python tools and the full test suite require Python 3.10+; image encoding requires Pillow. MTP probing requires PyUSB.

```sh
python3 -m unittest discover -s tests -v  # Python 3.10+ and Pillow
```

### License

Newly licensed project material is released under the [GR IV Project Noncommercial Source License 1.0](LICENSE). Commercial use requires written permission; this is not an OSI-approved open-source license. Revisions through [`a55a2c7`](https://github.com/radium-wang/ricoh-gr4-firmware-analysis-and-feature-expansion/tree/a55a2c7) and unchanged material from those revisions retain their Apache-2.0 grants. See [NOTICE](NOTICE) and [CONTRIBUTING.md](CONTRIBUTING.md).

Camera modifications carry a risk of data loss or malfunction. Results apply only to the documented test setups. No rights to third-party firmware, artwork or trademarks are granted.

---

## 中文

理光 GR 固件研究与关机图片工具，包含固件格式分析、USB/MTP 只读探测、工厂菜单记录，以及关机图片备份、替换和恢复脚本。

图片替换通过 SD 卡上的 TTL 脚本执行，无需刷写固件。仓库不提供固件、解包资源或图片素材。

### 机型与操作指南

| 机型 | 指南 | 验证情况 |
| --- | --- | --- |
| GR IV | [GR IV 系列](docs/gr4-family-shutdown-workflow.zh-CN.md) | 原始替换流程在一台机身上成功；机型识别在 1.11 上验证 |
| GR IV HDF | [GR IV 系列](docs/gr4-family-shutdown-workflow.zh-CN.md) | 机型识别和目标路径在 1.11 上验证 |
| GR IV Monochrome | [GR IV 系列](docs/gr4-family-shutdown-workflow.zh-CN.md) | 机型识别和目标路径已验证，精确固件版本未记录 |
| GR IIIx Urban Edition 1.60 | [Urban 流程](docs/gr3x-urban-160-shutdown-image.md) | 一台机身完成替换、恢复及完整读回校验 |

新增 GR IV 系列组合流程和 Urban 备份／恢复工具通过了离线测试，尚未整套上机验证。其他机型和固件版本未经验证。

### 使用

执行脚本前，请先阅读对应机型的指南。在电脑上生成工厂菜单入口文件：

```sh
# GR IV 系列
python3 tools/create_factory_entry.py ./entry

# GR IIIx Urban Edition 1.60
python3 tools/create_factory_entry.py ./entry-urban --model gr3x-urban-160
```

GR IV 系列指南包含自动识别、备份、替换和恢复步骤，使用 FAT32 卡及与原图字节数相同的 720×480 JPEG。Urban 使用独立的两阶段安装方法，不能直接运行 GR IV 模板。

替换前务必将原图另存到电脑，每台相机保留独立备份。执行后校验完整 SHA-256，并检查实际关机画面；空读回文件表示失败或未完成。结束后删除 `script/startup.ttl`，把 Script 设回 Disable。

原有普通 GR IV 的 `*-goodbye.ttl.example` 模板仍使用 `GBBACK.JPG`，旧用户可以继续使用。不要与系列模板的备份名混用，也不要用于 HDF／Monochrome。已经改过且没有原图备份的相机，再次备份无法找回原图。

### 工具与资料

- [固件与关机图研究报告](docs/firmware-and-shutdown-image-research.md)
- [Debug 模式与工厂菜单分析](docs/gr4-debug-mode-analysis.md)
- [GR IV 系列机型识别](docs/gr4-model-identification.md)
- [研究日志](docs/research-log.md)
- [tools/](tools/)：固件检查、USB/MTP 探测、JPEG 处理及脚本生成器
- [examples/](examples/)：TTL 模板

直接复制 TTL 模板无需 Python。入口生成和基础分析使用 Python 3；GR IV 系列 Python 工具及整套测试需要 Python 3.10+，图片编码需要 Pillow，MTP 探测需要 PyUSB。

```sh
python3 -m unittest discover -s tests -v  # Python 3.10+，需 Pillow
```

### 许可证

新授权的项目内容采用 [GR IV Project Noncommercial Source License 1.0](LICENSE)，商业使用须取得书面许可；这不是 OSI 定义下的开源许可证。截至 [`a55a2c7`](https://github.com/radium-wang/ricoh-gr4-firmware-analysis-and-feature-expansion/tree/a55a2c7) 的旧版本及其保留的未修改内容仍适用原有 Apache-2.0 授权。详见 [NOTICE](NOTICE) 和[贡献指南](CONTRIBUTING.md)。

改机存在数据丢失或设备故障风险，实测结果仅对应文档记录的环境。项目许可证不授予第三方固件、图稿或商标的使用权。
