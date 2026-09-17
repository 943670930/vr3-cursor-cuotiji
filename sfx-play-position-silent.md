# 听不到音效：先查播音世界坐标

- **日期**：2026-07-27
- **场景**：玩家右手掌心朝上播「默认完美格挡」；代码/日志显示已尝试播放，多次改路径/开关后仍像没声音。
- **结论**：不是「没播」，是**位置播错**——用了手/掌心附近，听感像无声或极远；改到**武器碰撞接触点**后立刻能听到。
- **以后触发**：同类「改了很多次还是没声音」时，优先核对：
  1. `TryPlayPositionalSFX` / 战斗音是否带 **worldPos**，该点是否在玩家头附近可听范围；
  2. 不要默认用手的 Transform/掌心当锚点；持武/互碰优先用 **Collision contact** 或武器刚体中心；
  3. 可临时在播音点挂世界字（如「碰撞点」）或加大 volume / minDistance 做定位验收。
- **相关**：`PlayerRightPalmUpPerfectBlockSfxHost`、`PhysicsCollisionHandler.NotifyWeaponCollisionContact` 写入链。
