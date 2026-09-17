# VR 调试方块：一只眼能看见、另一只眼看不见

- **日期**：2026-09-07（手伤害红块监控复现；同类问题此前在体素/幽灵瞄准等已踩过）
- **场景**：`PmPlayerHandDamageBoxVisualMonitor` 挥速达标后刷红色方块；头显上一眼有红块、另一眼整片没有。
- **结论**：不是位置/父节点问题，是**材质 Shader 不带 VR 立体宏**。运行时 `Shader.Find("Universal Render Pipeline/Unlit")` / 默认 `Unlit/Color` 在 OpenXR **Single Pass Instanced** 下常只渲一只眼。
- **正确做法**：
  1. 用 **`Vr3StereoUnlitShaderUtil.CreateMaterial(color)`**（底层 `Resources/VR3UnlitColorStereo` → `VR3/Unlit Color Stereo`）
  2. 或自写 shader 时照 `.cursor/rules/builtin-transparent-voxel-material.mdc`：**stereo multi_compile、`Cull Off`、frag 里 `UNITY_SETUP_STEREO_EYE_INDEX_POST_VERTEX`**
  3. **禁止**指望 `ZTest Always` + 高 `renderQueue` 修单眼；根因在 shader pass，不在深度状态
- **以后触发**：任意 VR 里 `CreatePrimitive` / 运行时新建半透明调试几何，先查是否走了 `Vr3StereoUnlitShaderUtil`；参考 `PmPlayerVoxelGhostBowAimOverlay`、`ArrowHitStackMarkerSystem`（叠伤红块）。
- **验收**：左右眼同时能看到同色块，无「一眼全没」。
