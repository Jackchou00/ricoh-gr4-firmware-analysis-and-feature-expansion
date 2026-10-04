<!-- License: see LICENSE (earlier Apache-2.0 grants remain in force) -->

# GR IIIx HDF 1.60 shutdown image / GR IIIx HDF 1.60 关机画面

## Result and scope

A shutdown image was replaced and displayed on **one GR IIIx HDF running firmware 1.60**. The original resource and final target were read back in full and checked byte-for-byte on a computer. The user reported that the custom image remained visible after returning `Script` to `Disable` and power-cycling the camera. No firmware was changed. This result does not establish compatibility with another HDF body, a standard GR IIIx, an Urban Edition, or another firmware version.

关机图片已在**一台运行固件 1.60 的 GR IIIx HDF** 上替换并正常显示。原图、机内备份、临时文件和最终目标均完整读回电脑并逐字节核对。用户确认将 `Script` 改回 `Disable` 并重新开关机后，新图仍正常显示。整个流程没有刷写固件。该结果不能证明其他 HDF、普通 GR IIIx、Urban Edition 或其他固件版本兼容。

## Device-specific observations / 本机观察

| Item / 项目 | Observation / 观察 |
| --- | --- |
| Body and firmware / 机型与固件 | GR IIIx HDF, menu version 1.60 / GR IIIx HDF，菜单版本 1.60 |
| Factory-menu entry / 工厂菜单入口 | `00078490.609` + `DEVELOP.MOD`; confirmed on this body only / 仅在本机确认可用 |
| Shutdown resource / 关机资源 | `B:\Resource\Jpeg\GoodBye.jpg` |
| Original JPEG / 原图 | 720×480, baseline JPEG, 7,264 bytes / 720×480，baseline JPEG，7,264 字节 |
| Internal original backup / 机内原图备份 | `B:\Resource\Jpeg\HD41OL.JPG` |
| Internal temporary image / 机内临时图 | `B:\Resource\Jpeg\HD81NW.JPG` |

The HDF body also exposed `GB_Urban.jpg`, `GB_Diary.jpg` and `GB_ING.jpg` under `B:\Resource\Jpeg`. Reading those resources did not establish them as the HDF shutdown target. Replacing `GoodBye.jpg`, reading the installed bytes back, and observing the changed shutdown screen confirmed the active path on this body.

本机还可从 `B:\Resource\Jpeg` 读出 `GB_Urban.jpg`、`GB_Diary.jpg` 和 `GB_ING.jpg`。单独读到这些文件不能证明它们是 HDF 的关机目标；在本机替换 `GoodBye.jpg` 并观察到关机画面改变，才确认了活动路径。

## Verified procedure / 实测流程

The operation used three separate startup scripts. Keep a computer copy of every readback and check the complete content before moving to the next step.

整个操作分三段运行启动脚本。每段都要把读回文件保存到电脑，并在继续前核对完整内容。

1. **Backup.** The first script copied the original from internal `B:` to SD-card `C:`, created `HD41OL.JPG` on internal `B:`, and copied that backup back to `C:`. Both SD readbacks were 7,264 bytes and byte-for-byte identical to the original resource.
2. **Temporary image.** The second script created a fresh `HD81NW.JPG`, extended it to 7,264 bytes, and wrote the candidate JPEG with `fileseek` and short `filewrite` numeric literals. It then copied the temporary file and the pre-install originals to the card. The original and temporary readbacks matched their local references byte-for-byte. The installed image bytes and user artwork are not included in this repository.
3. **Install.** Only after those checks, a separate short script copied `HD81NW.JPG` to `GoodBye.jpg`, copied the target back to SD, and checked the target attributes and size. The final 7,264-byte readback matched the candidate byte-for-byte. The user confirmed the camera displayed it.

The backup example at [`examples/gr3x-hdf-160-backup.ttl.example`](../examples/gr3x-hdf-160-backup.ttl.example) contains the first-step script used in the experiment. The install example at [`examples/gr3x-hdf-160-install.ttl.example`](../examples/gr3x-hdf-160-install.ttl.example) contains the size-guarded install and readback step; it expects a candidate already staged as `HD81NW.JPG`. The computer-side candidate and its generated payload-write script were specific to user-supplied artwork, so they are not included. The recovery example at [`examples/gr3x-hdf-160-restore.ttl.example`](../examples/gr3x-hdf-160-restore.ttl.example) uses the camera's own verified `B:` backup; its guarded wrapper was **not** run on-camera. The install script's automatic rollback path was also not exercised.

备份示例 [`examples/gr3x-hdf-160-backup.ttl.example`](../examples/gr3x-hdf-160-backup.ttl.example) 是本次使用的第一步脚本。安装示例 [`examples/gr3x-hdf-160-install.ttl.example`](../examples/gr3x-hdf-160-install.ttl.example) 包含安装和读回步骤，要求候选图已预先写入 `HD81NW.JPG`。候选图和按用户图稿生成的 payload 写入脚本没有放入仓库。恢复示例 [`examples/gr3x-hdf-160-restore.ttl.example`](../examples/gr3x-hdf-160-restore.ttl.example) 从本机已验证的 `B:` 备份恢复，但这个带保护的恢复脚本**没有在相机上运行**；安装脚本的自动回滚分支也没有实测。

## Limits and recovery / 限制与恢复

- The reported original size and paths are observations from one body. Before writing another body, read and back up that body's own resource; do not assume the same path, length or attributes.
- Equal length and desktop JPEG decoding are necessary checks, not proof that a camera decoder will accept an arbitrary image. Confirm a complete readback and the actual shutdown display.
- The Urban Edition generator pins an Urban original size and SHA-256. It is **not** a GR IIIx HDF generator and must not be used unchanged for HDF.
- The first attempted backup script produced no outputs. A task-created macOS `._startup.ttl` AppleDouble file was present. Removing task-created metadata was followed by a successful read-only diagnostic and then a successful backup; the cause of the first failure was not conclusively established. Before a run, inspect `script/` and remove only known task-created `._*` files.
- Recovery requires the original from the same body. Verify the internal backup against the computer copy before running a restore script. The restore wrapper in this report is not camera-qualified.
- Remove `script/startup.ttl`, return `Script` to `Disable`, confirm ordinary power cycles, and clean only task files from the card. Do not remove user photos.

- 原图长度和路径仅来自一台机身。写另一台之前，先读取并备份那台机身自己的资源，不要假设路径、长度或属性相同。
- 等长和电脑端 JPEG 解码只是必要检查，不能保证相机解码器接受任意图像；还需完整读回并检查实际关机画面。
- Urban Edition 生成器固定了 Urban 原图长度和 SHA-256，**不是** GR IIIx HDF 生成器，不能不经修改用于 HDF。
- 首次运行备份脚本没有产出文件。当时卡上有本任务产生的 macOS `._startup.ttl` AppleDouble 文件；清理已知任务元数据后，只读诊断与备份脚本运行成功，但不能断定这就是首次失败的原因。运行前检查 `script/`，只清理确认属于本任务的 `._*` 文件。
- 恢复必须使用同一台机身的原图。运行恢复脚本前，先将机内备份与电脑副本完整核对。本报告中的恢复包装脚本未在相机上验证。
- 完成后移除 `script/startup.ttl`，将 `Script` 改回 `Disable`，确认正常开关机，再清理卡上的任务文件；不要删除用户照片。
