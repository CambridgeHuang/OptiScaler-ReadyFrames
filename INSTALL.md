# 安装与使用 / Install & Usage

适用版本 / Applies to: `v0.1.1`，基于上游 `b7b07d2d` / based on upstream `b7b07d2d`

---

## 0. 先确认你属于哪种情况 / Which case are you in?

| 情况 / Case | 怎么做 / What to do |
|---|---|
| **A. 已经在用 OptiScaler** | 只需要下载 `OptiScaler.dll`，改名成你现在用的那个文件名，覆盖即可。<br>**A. Already running OptiScaler** — download `OptiScaler.dll` only, rename it to the filename you already use, and overwrite. |
| **B. 全新用户，从没装过 OptiScaler** | 先按 OptiScaler 官方发布包装好（官方包里有 `OptiScaler.ini`、`amd_fidelityfx_framegeneration_dx12.dll` 等 FSR FG 运行时，本 Release 不包含这些），再用本 DLL 覆盖。<br>**B. Fresh install** — install OptiScaler from the official release first (it ships `OptiScaler.ini` and the FSR FG runtime such as `amd_fidelityfx_framegeneration_dx12.dll`, which this release does not include), then overwrite with this DLL. |

> 说明：本 Release 只提供 `OptiScaler.dll` 一个文件。FSR FG / XeFG 所需的运行时 DLL
> 和配置文件来自官方包，所以全新用户必须先装官方版。
>
> Note: this release ships a single file, `OptiScaler.dll`. The runtime DLLs and
> configuration file required for FSR FG / XeFG come from the official package, so
> a fresh install must start from the official release.

---

## 1. 备份现有文件 / Back up first

把游戏目录里现有的那个注入 DLL 改名成 `.bak` 备份，不要直接删。

Rename the injector DLL currently in the game folder to a `.bak` file. Do not
delete it — you will need it to roll back.

```
<游戏目录 / game folder>\dxgi.dll  →  dxgi.dll.bak
```

---

## 2. 确认注入名 / Pick the filename

改名成**你当前正在用的那个文件名**，不要换名字。常见的有：

Rename it to **whatever filename you already use** in that game folder — do not
switch to a different name. Common ones are:

| 常见注入名 / Common name | 备注 / Notes |
|---|---|
| `dxgi.dll` | 最通用 / most common |
| `nvngx.dll` | 配合 `nvngx_dlss.dll` 的替换路径 / used with a `nvngx_dlss.dll` replacement |
| `winmm.dll` | 部分游戏 / some titles |
| `dbghelp.dll` | 部分游戏 / some titles |
| `version.dll` | 部分游戏 / some titles |

> 同一个游戏目录里只保留一个注入文件名，不要同时放多个。
>
> Keep a single injector filename in one game folder — do not place several at once.

---

## 3. 改名并覆盖 / Rename & overwrite

1. 把下载的 `OptiScaler.dll` 改名成第 2 步确认的名字（例如 `dxgi.dll`）。
2. 复制进游戏可执行文件所在目录，覆盖同名文件。
3. 启动游戏。

1. Rename the downloaded `OptiScaler.dll` to the name you confirmed in step 2
   (for example `dxgi.dll`).
2. Copy it into the folder containing the game executable and overwrite.
3. Launch the game.

---

## 4. 配置 Ready Frames / Configure the gate

打开游戏目录下的 `OptiScaler.ini`，在 `[DLSSG]` 段里加一行：

Open `OptiScaler.ini` in the game folder and add one line under `[DLSSG]`:

```ini
[DLSSG]
RequiredReadyFrames=auto
```

| 取值 / Value | 含义 / Meaning |
|---|---|
| `auto` | 默认阈值 30 个连续就绪帧 / default threshold of 30 consecutive ready frames |
| `1`–`300` | 指定连续就绪帧阈值 / explicit consecutive-frame threshold |
| `0` | 关闭闸门，回到上游原始行为，用于对照 / gate disabled, upstream behaviour, for comparison |

> 进游戏后按 `Insert` 可以打开 OptiScaler Overlay 查看帧生成状态。
>
> Press `Insert` in game to open the OptiScaler overlay and check frame-generation state.

---

## 5. ⚠️ 版本绑定 / Version lock

OptiScaler 没有官方更新器，"更新"= 下载官方新版压缩包覆盖安装目录。覆盖会连同本
DLL 一起替换掉；如果之后又把本 DLL 覆盖回去，实际运行的仍然是 `b7b07d2d` 这个
构建，官方新版里的修复和改动不会生效。两者不能叠加：

- 想用 Ready Frames → 停在这个构建，或使用本仓库发布的最新版本（我会持续跟进
  上游，基于更新提交重新构建并发布在本仓库）
- 想跟进官方新版 → 就得放弃 Ready Frames

另外，新版新增的 ini 键本构建不认识，会被当作未知键忽略。

OptiScaler has no official updater. "Updating" means overwriting the folder with
a newer official package, which replaces this DLL too. Copying this DLL back
afterwards puts you back on build `b7b07d2d` — fixes from the newer official
version will **not** be active. The two cannot be combined:

- Want Ready Frames → stay on this build, or use the newest release from this
  repository (I keep rebasing on upstream and publish new builds here).
- Want the newest upstream → give up Ready Frames.

Also note: INI keys added by newer versions are unknown to this build and will be
silently ignored.

---

## 6. 校验文件 / Verify the file

```powershell
Get-FileHash .\OptiScaler.dll -Algorithm SHA256
```

```bash
sha256sum OptiScaler.dll
```

`v0.1.1` 的 DLL 与 `v0.1` 完全一致，SHA-256 应为：

The `v0.1.1` DLL is byte-identical to `v0.1`. Expected SHA-256:

```
1dd10e7e2966c56126cfa443c66e7909431e4d69eaf9390a4ba786827433b598
```

---

## 7. 回滚 / Rollback

把 `.bak` 文件改回原名即可。`OptiScaler.ini` 里新增的那一行不影响原版运行，也可以
一并删掉。

Rename the `.bak` file back to its original name. The added `OptiScaler.ini` line
does not affect the original build, but you may delete it as well.

---

## 8. 测试情况与免责 / Tested & disclaimer

实测配置：AMD Ryzen 7 9800X3D / 32 GB DDR5-6000 CL28 / Radeon RX 9070 XT
（Gigabyte AORUS）/ AMD Adrenalin 26.11.2。在《燕云十六声》中国大陆服务器中，
多场景切换时帧生成表现相较未启用 Ready Frames 更稳定，崩溃与闪退次数明显减少。
结果仅覆盖 FSR FG 输出路径及上述配置；XeFG 路径与其他显卡、驱动组合未验证。本
构建不构成任何保证。

Tested on: AMD Ryzen 7 9800X3D / 32 GB DDR5-6000 CL28 / Radeon RX 9070 XT
(Gigabyte AORUS) / AMD Adrenalin 26.11.2, in Where Winds Meet (mainland China
servers). Frame generation across scene transitions was more stable than without
Ready Frames, with noticeably fewer crashes and forced closes. Results cover the
FSR FG output path and the configuration above only; the XeFG path and other
GPU/driver combinations are unverified. No warranty of any kind.
