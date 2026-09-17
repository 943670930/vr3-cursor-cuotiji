# 右手距真实地面高度读错 / Wrong right-hand height vs real floor

## 问题 / Problem

要显示右手相对**真实地面**的高度（例如控制器放桌上约 0.8m）。用 `trackingContainer` 逆变换、`follow.localPosition.y`、Unity 世界 `position.y`、头显 local Y 等，数值要么不动、要么差约 0.6m、要么变成头高。

Need right-hand height above the **real / guardian floor** (e.g. ~0.8m when the controller is on a table). Inverse `trackingContainer`, `follow.localPosition.y`, Unity world `position.y`, or HMD local Y stay frozen, miss by ~0.6m, or report head height.

## 解决方案 / Solution

工程里有两套竖直参考：XR Floor（相对校准地面）和 AutoHand / `XrOriginFloorTrackingBootstrap`（原点贴到**游戏 mesh 地面**）。游戏世界 / AH 链上的 Y 不是真实地面高度。

There are two vertical references: XR Floor (guardian-calibrated) and AutoHand / `XrOriginFloorTrackingBootstrap` (origin snapped to **in-game mesh floor**). Y on the game-world / AutoHand chain is not real-floor height.

| 误用 / Wrong read | 实际量的是 / What you actually measure |
|---|---|
| `trackingContainer.InverseTransformPoint(hand).y` | AH 贴游戏地后的容器，易 +~0.6m / Container after AH snap; often +~0.6m |
| `handRight.follow.localPosition.y` | prefab 固定偏移，不随手柄变 / Prefab local offset, does not track the controller |
| 手世界 Y / Hand world Y | 场景高度 / Scene height |
| `headCamera.localPosition.y` 当手高 / as hand height | 头显身高 / HMD height |

**正确读法 / Correct read（已验证 / verified）：**

- 前提 / Require: `XrOriginFloorTrackingBootstrap.IsFloorTrackingApplied == true`
- 右手距真实地面 / Right hand vs real floor: `InputDevices.GetDeviceAtXRNode(XRNode.RightHand)` → `CommonUsages.devicePosition.y`（需 `isTracked`）
- 头显距真实地面是另一条链：`headCamera.transform.localPosition.y`，不要和手公式混用。  
  HMD vs real floor is a different chain: `headCamera.transform.localPosition.y`. Do not mix it with the hand formula.
- 读不到就显示「获取不到」，不要 silent fallback 到另一条链。  
  If unread, show “unavailable”; do not silently fall back to another chain.
