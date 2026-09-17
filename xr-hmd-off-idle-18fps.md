# 头显关机锁 18 帧 / HMD off locks Editor Play at 18 FPS

## 问题 / Problem

Unity Editor Play（含空 `New Scene`）稳在约 18 FPS。Profiler 主耗时在 `XR.Display.PopulateNextFrameDesc` / `SubmitCurrentFrame`，`xrRefreshHz=18`，眼交换链仍是 2352×2352。C# `BehaviourUpdate` 只有几毫秒；关场景、关 Game View、压 viewport 都不升帧。

Unity Editor Play (including an empty `New Scene`) sits at about 18 FPS. Profiler time is in `XR.Display.PopulateNextFrameDesc` / `SubmitCurrentFrame`, `xrRefreshHz=18`, eye swapchain still 2352×2352. C# `BehaviourUpdate` is only a few ms. Disabling scene geometry, Game View, or viewport scale does not raise FPS.

## 解决方案 / Solution

头显关机或未重连，头显里看不到当前 Play 画面。OpenXR 合成器掉拍锁档（90÷5=18），不是场景画太多，也不是逻辑卡。

The headset is powered off or not reconnected, so it never shows the current Play view. The OpenXR compositor locks to a reduced cadence (90÷5=18). This is not a heavy scene and not a C# stall.

**做法 / Do this：** 开机头显 → 重连 PICO Connect / SteamVR → 确认头显里能看到游戏画面。帧数恢复后再查代码。不要先改眼分辨率、MSAA、额外相机或 DX12。

Power on the HMD → reconnect PICO Connect / SteamVR → confirm the headset shows the game. Only then debug code. Do not start with eye resolution, MSAA, extra cameras, or DX12.
