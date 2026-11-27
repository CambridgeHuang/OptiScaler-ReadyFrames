# OptiScaler Ready Frames (based on `b7b07d2d`)

Unofficial OptiScaler build with a configurable **Ready Frames** gate for DX12
Streamline 2 DLSS-G input paired with FSR FG or XeFG output. FSR FG was the first
implementation priority; XeFG uses the same shared path. The gate waits for a
number of consecutive frames with constants, depth, and motion-vector inputs
before allowing frame generation to activate.

This build is **not** a general-purpose crash fix. Its tested scope is described
below. It is based on upstream OptiScaler commit
`b7b07d2d19ba1b19fab07f6189d239dd49ea4552` (2026-11-23).

## 我该下载哪个 / Which file do I need?

| 适用人群 / Audience | 下载 / Download |
|---|---|
| 已经装好 OptiScaler，只需替换 DLL<br>Already running OptiScaler — just swap the DLL | `OptiScaler.dll` |
| 想核对源码的人（GPL 合规用途，普通用户不需要）<br>Reviewing the source (GPL compliance; end users don't need it) | [v0.1 Release 的源码附件 / source asset in the v0.1 release](https://github.com/CambridgeHuang/OptiScaler-ReadyFrames/releases/tag/readyframes-b7b07d2d) |

安装步骤见 [`INSTALL.md`](INSTALL.md) / Step-by-step instructions: see
[`INSTALL.md`](INSTALL.md).

## Release files

| 文件 / File | 说明 / Notes |
|---|---|
| `OptiScaler.dll` | 已有 OptiScaler 的用户只需这一个。使用时改名成**你当前正在用的那个文件名**，不要换名字。<br>Existing users: this is the only file you need. Rename it to **whatever filename you already use** in the game folder — do not switch names. |
| `ReadyFrames-b7b07d2d.patch` | 相对上游 `b7b07d2d` 的改动，便于复核。<br>Diff against upstream `b7b07d2d`, for review. |
| `OptiScaler-ReadyFrames-source-b7b07d2d.zip` | 完整对应源码，GPL-3.0 合规用途，托管在 v0.1 Release。<br>Complete corresponding source for GPL-3.0 compliance, hosted in the v0.1 release. |

常见注入名（仅供参考，按你原来的来）/ Common injector names (reference only — keep the
one you already use):
`dxgi.dll` · `nvngx.dll` · `winmm.dll` · `dbghelp.dll` · `version.dll`

## 测试情况与适用范围 / Tested & Scope

**实测配置 / Tested on**：AMD Ryzen 7 9800X3D / 32 GB DDR5-6000 CL28 / Radeon RX
9070 XT（Gigabyte AORUS）/ AMD Adrenalin 26.11.2。

在《燕云十六声》中国大陆服务器中实测：多场景切换时帧生成表现相较未启用 Ready
Frames 更稳定，崩溃与闪退次数明显减少。以上结果仅覆盖 FSR FG 输出路径及上述配置。

Measured in Where Winds Meet (mainland China servers): frame generation across
scene transitions was more stable than without Ready Frames, with noticeably
fewer crashes and forced closes. These results cover the FSR FG output path and
the configuration above only.

**适用范围（推测，未经证实）/ Scope (speculative, unverified)**
本构建并非专为《燕云十六声》制作。凡是在游戏中会频繁切换帧生成状态的作品（反复
进出菜单、切换画质档、频繁过场），理论上都可能受益于同样的「连续就绪帧」闸门
逻辑，《燕云十六声》只是一个典型样本。假设《燕云十六声》国际服与中国大陆服务器
在底层渲染与帧生成实现上不存在重大差异，理论上也应获得相同效果，但未经验证。
XeFG 输出路径使用同一套共享逻辑，理论上同样受益，同样未经验证。

This build is not made specifically for Where Winds Meet. Any title that toggles
frame generation frequently — repeated menu entry, quality-preset switches,
frequent cutscenes — could in principle benefit from the same "consecutive ready
frames" gate; Where Winds Meet is merely a representative sample. If the
international release of Where Winds Meet does not differ substantially from the
mainland build in its underlying rendering or frame-generation implementation,
the same effect would be expected — however, this is unverified. The XeFG output
path shares the same logic and would in principle benefit as well; also
unverified.

## Configuration

Under `[DLSSG]` in `OptiScaler.ini`, set `RequiredReadyFrames`:

- `auto`: default of 30 consecutive ready frames.
- `1` through `300`: explicit consecutive-frame threshold.
- `0`: disable the gate and restore the original behavior for comparison.

## Build

Built with Visual Studio 2022 Build Tools v143 and Windows SDK 10.0.26100,
Release x64. Reproduce with the complete source archive or apply the patch to the
exact upstream commit, then build `OptiScaler.sln`.

## License and attribution

OptiScaler is licensed under the GNU General Public License v3.0. The complete
corresponding modified source is included in the source archive, and the patch is
provided for review. The upstream `LICENSE` and dependency notices are included.
This is an unofficial community build and is not affiliated with the OptiScaler
project.

The GPL-3.0 license text is reproduced in `LICENSE`. This repository does not add
any restriction that the GPL does not permit to the upstream work; the paragraph
below is a non-binding request, not an additional license term. No NVIDIA
proprietary binaries or model weights are included.

### 关于商业化 / Commercial use

非约束性请求（**不是许可条款**，不改变 GPL-3.0 授予的任何权利）：如果你想付费
转售本构建或将其商业化，请先取得作者的明确授权；在未取得授权前，请不要付费转售
或商业化。

Non-binding request, **NOT a license term**: paid resale or commercial use of
this build requires the author's explicit permission.

说明 / Note：OptiScaler is licensed under GPL-3.0; GNU GPLv3 section 7 forbids
additional restrictions, so the paragraph above is a statement of position and is
not enforceable as a condition. Your rights under GPL-3.0 are unaffected. The
upstream project likewise states that OptiScaler is free and that any monetary
requirement is a scam.
