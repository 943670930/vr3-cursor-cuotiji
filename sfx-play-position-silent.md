# 听不到音效：先查播音坐标 / Silent SFX: check 3D play position first

## 问题 / Problem

代码/日志显示已尝试播放（例如右手掌心朝上的格挡音），多次改路径、开关后仍像没声音。

Code/logs show playback was attempted (e.g. palm-up perfect-block SFX), but after many path/toggle changes it still sounds silent.

## 解决方案 / Solution

不是「没播」，是**世界坐标播错**——用手/掌心附近时，听感像无声或极远。改到**武器碰撞接触点**后立刻能听到。

It did play. The **world position** was wrong — near the hand/palm it sounds mute or infinitely far. Playing at the **weapon collision contact** made it audible immediately.

**做法 / Do this：**

1. `TryPlayPositionalSFX` / 战斗音必须带听得见的 **worldPos**（靠近玩家头）。  
   Positional combat SFX must pass an audible **worldPos** (near the player head).
2. 不要默认用手 Transform/掌心当锚点；持武互碰优先 **Collision contact** 或武器刚体中心。  
   Do not default to the hand/palm transform; prefer collision contact or the weapon rigidbody center.
3. 可临时在播音点挂标记或加大 volume / minDistance 做验收。  
   Temporarily mark the play point or raise volume / minDistance to verify.

相关 / Related: `PlayerRightPalmUpPerfectBlockSfxHost`, `PhysicsCollisionHandler.NotifyWeaponCollisionContact`.
