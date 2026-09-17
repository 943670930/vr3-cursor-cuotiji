# 头显关机 / 看不到游戏画面 → Editor Play 锁 18 帧

## 现象

Unity Editor Play（含空 `New Scene`）稳在约 18 FPS。Profiler 主耗时在 `XR.Display.PopulateNextFrameDesc` / `SubmitCurrentFrame`，`xrRefreshHz=18`，眼交换链仍是 2352×2352。BehaviourUpdate 只有几毫秒；关场景几何、关 Game View、压 viewport 都不升帧。

## 错因

头显关机或未重连，头显里看不到当前 Play 画面。OpenXR 合成器按掉拍锁档（90÷5=18），不是场景画太多，也不是 C# 卡逻辑。

## 以后

Editor Play 空场景也 18 帧、卡在 Submit/Populate 时：**先开机头显 → 重连 PICO Connect/SteamVR → 确认头显里能看到游戏画面**。帧数恢复后再查代码。不要先改眼分辨率、MSAA、额外相机或 DX12。
