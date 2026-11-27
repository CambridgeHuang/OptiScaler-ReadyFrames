# OptiScaler Ready Frames (based on `b7b07d2d`)

This is an unofficial OptiScaler build with a configurable Ready Frames gate for DX12 Streamline 2 DLSS-G input paired with FSR FG or XeFG output. FSR FG was the first implementation priority; XeFG uses the same shared path. The gate waits for consecutive frames with constants, depth, and motion-vector inputs before allowing frame generation to activate.

This build has **not** been tested in-game and is not a confirmed crash fix. It is based on upstream OptiScaler commit `b7b07d2d19ba1b19fab07f6189d239dd49ea4552` (2026-11-23).

## Release files

- `dxgi.dll`: unsigned Release x64 OptiScaler DLL.
- `ReadyFrames-b7b07d2d.patch`: source changes against the exact upstream commit.
- `OptiScaler-ReadyFrames-source-b7b07d2d.zip`: complete modified source tree and pinned dependencies, including upstream license and notices. Only generated build outputs in `x64/` and `OptiScaler/x64/` are omitted.
- `SHA256SUMS.txt`: SHA-256 checksums for the release assets.

## Configuration

Under `[DLSSG]` in `OptiScaler.ini`, set `RequiredReadyFrames`:

- `auto`: default of 30 consecutive ready frames.
- `1` through `300`: explicit consecutive-frame threshold.
- `0`: disable the gate and restore the original behavior for comparison.

## Build

Built with Visual Studio 2022 Build Tools v143 and Windows SDK 10.0.26100, Release x64. Reproduce with the complete source archive or apply the patch to the exact upstream commit, then build `OptiScaler.sln`.

## License and attribution

OptiScaler is licensed under the GNU General Public License v3.0. The complete corresponding modified source is included in the source archive and the patch is provided for review. The upstream `LICENSE` and dependency notices are included. This is an unofficial community build and is not affiliated with the OptiScaler project.

The GPL-3.0 license text is reproduced in `LICENSE`. This repository does not add a non-commercial restriction to the GPL-covered OptiScaler work. No NVIDIA proprietary binaries or model weights are included.

## 中文说明

这是基于 OptiScaler 上游 commit `b7b07d2d19ba1b19fab07f6189d239dd49ea4552` 的非官方 Ready Frames 构建，面向 DX12 + Streamline 2 DLSS-G 输入配合 FSR FG / XeFG 输出。实现和测试优先级为 **FSR FG 第一、XeFG 第二**。Ready Frames 在连续若干帧的常量、深度和运动矢量输入齐全后，才允许开启帧生成。

本构建**尚未进行游戏内测试**，不能视为已确认修复闪退。源码包包含对应修改后的完整源码、固定版本依赖、上游 GPL-3.0 许可证及依赖声明。

配置位于 `OptiScaler.ini` 的 `[DLSSG]` 段：`RequiredReadyFrames=auto` 默认 30 帧；可设为 `1`–`300`，设为 `0` 则关闭闸门、恢复原行为以供对照。

上游 OptiScaler 使用 GPL-3.0。公开分发 DLL 时，需同时提供对应源码；本仓库随附完整源码包和补丁。**GPL-3.0 明确的许可条款适用；本仓库不对 GPL 覆盖的 OptiScaler 部分额外添加“禁止商用”限制。** 本项目为非官方构建，与 OptiScaler 官方项目无隶属关系。
