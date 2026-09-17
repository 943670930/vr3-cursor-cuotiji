# 错题集 / Mistake notebook

本目录是**踩坑备忘**，不是 Cursor 强制规则（不要放进 `.cursor/rules/`）。

This folder is a **pitfall notebook**, not Cursor project rules (do not put it in `.cursor/rules/`).

## 工程环境 / Project context

Unity VR 游戏。玩家交互用 **Auto Hand**；unit 身体/布娃娃用 **PM（PuppetMaster）**。XR 走 OpenXR（本机常见 PICO + SteamVR）。

Unity VR game. Player interaction uses **Auto Hand**. Units use **PM (PuppetMaster)** for body/ragdoll. XR is OpenXR (here often PICO + SteamVR).

同类问题反复改代码仍不对时，先对照这里的**问题 + 解决方案**。

When the same class of bug keeps surviving code changes, match **Problem** and **Solution** here first.

| 条目 / Entry | 问题 / Problem | 解决方案 / Solution |
|------|--------|----------|
| [sfx-play-position-silent.md](./sfx-play-position-silent.md) | 代码显示已播，仍像没声音 / Playback runs but sounds silent | 查三维播音世界坐标，勿用手位置 / Use world contact pos, not the hand |
| [new-log-missing-not-runtime-missing.md](./new-log-missing-not-runtime-missing.md) | 新 log 没有 / New diagnostic log missing | 不等于玩法没发生；先查 CS 红与旧 DLL / Gameplay still happened; check compile errors and stale DLL |
| [vr-right-hand-real-floor-height.md](./vr-right-hand-real-floor-height.md) | 右手距真实地面高度读错 / Wrong height vs real floor | 读 XR Floor `devicePosition.y` / Read XR Floor `devicePosition.y` |
| [vr-debug-cube-one-eye-missing.md](./vr-debug-cube-one-eye-missing.md) | 调试方块一眼有一眼无 / Debug cube visible in one eye only | 用 `Vr3StereoUnlitShaderUtil` / Use `Vr3StereoUnlitShaderUtil` |
| [xr-hmd-off-idle-18fps.md](./xr-hmd-off-idle-18fps.md) | Editor Play 空场景也 18 帧 / Empty Play stuck at 18 FPS | 开机头显、重连，直到头显能看到游戏 / Power on HMD, reconnect, until the headset shows the game |
