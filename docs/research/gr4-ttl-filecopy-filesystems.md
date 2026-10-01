# GR IV TTL file operations: filesystem-dependent failures

Source: [Issue #1](https://github.com/radium-wang/ricoh-gr4-firmware-analysis-and-feature-expansion/issues/1), reported by Daniel-Shi-233, 2026-09-30. This summarizes contributor evidence, not an independent reproduction.

## Device observations

On GR IV 1.11:

| Operation | 256 GB exFAT | 32 GB FAT32 |
| --- | --- | --- |
| Startup script | Executed | Executed |
| Original resource backup | Verified | Verified |
| SD-to-SD text/JPEG copy | Empty output | Verified |

FAT32 supported a verified larger replacement. This compares different cards; it does not isolate filesystem causality or establish universal exFAT failure. Ordinary photography still worked on the exFAT card.

## Static findings reported in the issue

| Runtime address, GR IV 1.11.10.7 | Finding |
| --- | --- |
| `0x53B1827C` | Copy handler queries length after opening files |
| `0x538F172C` | Length callback reopens source; failure can leave zero size |
| `0x538F15B8` | Read callback does not obtain actual byte count |

Repeated-open failure remains a hypothesis. An EOF-driven `fileconcat` attempt produced abnormal growth; do not recommend it as a workaround.

## Implications

Preflight SD-to-SD copies before internal writes. Verify size, full SHA-256 and display; completion alone proves nothing. An unchanged resource readback is not evidence of JPEG incompatibility.
