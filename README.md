# 错题集（非规则）

本目录是**踩坑备忘**，**不是** Cursor 项目规则（不放进 `.cursor/rules/`，不自动当强制约定）。

用途：同类问题反复改代码仍不对时，先对照这里的已知根因，少绕弯。

| 条目 | 一句话 |
|------|--------|
| [sfx-play-position-silent.md](./sfx-play-position-silent.md) | 多次改仍听不到音效 → 先查三维播音位置（尤其别错用手的位置） |
| [new-log-missing-not-runtime-missing.md](./new-log-missing-not-runtime-missing.md) | 新埋点 log 没有 ≠ 用户看见的运行时没发生；先查 CS 红 / 本次 Play 是否加载新 DLL |
| [vr-right-hand-real-floor-height.md](./vr-right-hand-real-floor-height.md) | 右手真实地面高度须读 XR Floor 的 `devicePosition.y`，勿用 AH/世界 Y / follow.local |
| [vr-debug-cube-one-eye-missing.md](./vr-debug-cube-one-eye-missing.md) | VR 调试方块一眼有、一眼无 → 用 `Vr3StereoUnlitShaderUtil`，勿 `Shader.Find` 普通 Unlit/URP |
| [xr-hmd-off-idle-18fps.md](./xr-hmd-off-idle-18fps.md) | Editor Play 空场景也 18 帧、卡在 XR Submit → 先开机头显并重连，直到头显能看到游戏画面 |
