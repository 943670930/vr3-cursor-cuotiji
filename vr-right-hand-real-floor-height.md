# VR 右手「距真实地面高度」读错坐标系

## 现象

Editor 设置 UI 背面要显示**右手相对真实地面**的高度（例如控制器放桌上约 0.8m）。  
试过 `trackingContainer` 逆变换、`follow.localPosition.y`、Unity 世界 `position.y`、头显 `headCamera.localPosition.y`、多条 fallback 对照表——数值要么不动、要么差 0.6m 量级，要么全是头高。

## 错因

VR3 运行时叠了两套竖直参考：

1. **XR Floor 空间**（OpenXR / `InputDevices`，Y = 相对 guardian 校准地面）  
2. **AutoHand + `XrOriginFloorTrackingBootstrap`**（把 tracking 原点贴到**游戏 mesh 地面**，改写了 rig 世界 Y）

把 **游戏世界 / AH Transform 链** 里的 Y 当成「真实地面高度」，量的是相对游戏地或 prefab 偏移，不是相对真实地面。

| 误用读法 | 实际量在什么 |
|---|---|
| `trackingContainer.InverseTransformPoint(手世界).y` | AH 贴游戏地后的容器；follow 常不在该链上 → 易 **+~0.6m** |
| `handRight.follow.localPosition.y` | prefab 固定 local 偏移 → **不随手柄变** |
| `RightControllerGO` / follow **世界 Y**（或减 tracking 地面） | Unity **场景高度** |
| `headCamera.localPosition.y` 当手高 | **头显身高**（~1.1–1.2m），不是手 |

## 正确做法（本项目已验证）

**右手距真实地面**（单一值，无 fallback）：

- 前提：`XrOriginFloorTrackingBootstrap.IsFloorTrackingApplied == true`（XR 激活时）  
- 读：`InputDevices.GetDeviceAtXRNode(XRNode.RightHand)` → `CommonUsages.devicePosition.y`（需 `isTracked`）  
- 标签：`xrRightDeviceY`（曾用于 Editor 背面 HUD / 扳机采样，已移除）

**头显**距真实地面仍是另一条链（不要混用手公式）：

- `headCamera.transform.localPosition.y`（+ Floor 门控）→ `TryGetMeters` / `PlayerEnterHeightCubeHost`

历史对照：`PlayerRealFloorRightTriggerSample` 写盘里 **`xrRightY≈0.9`** 时手在桌上，而 **`rightWorldY≈1.3+`** 明显偏大——应用前者，不用后者。

## 以后

- 要「**真实 / guardian 地面**」→ **XR `devicePosition.y`（Floor 模式）**  
- 要「**游戏 mesh 地面**」→ 另写需求，用 `TryGetGameFloorWorldY` / 世界 Y 差，**不要**和真实地面 HUD 混读  
- 禁止再堆 Transform 逆变换 + XR + 世界 Y 的「对照表」猜公式；先定量的是哪套地面，再选唯一读法  
- 读不到 → 显示「获取不到」，不要 silent fallback 到另一条链
