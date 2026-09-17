# VR 调试方块一眼有一眼无 / VR debug cube missing in one eye

## 问题 / Problem

运行时刷的调试方块（如挥速达标红块）在头显上一只眼看得到，另一只眼整片没有。位置和父节点往往是对的。

A runtime debug cube (e.g. swing-speed red box) is visible in one HMD eye and completely missing in the other. Position and parent are often already correct.

## 解决方案 / Solution

材质 Shader 不带 VR 立体宏。`Shader.Find("Universal Render Pipeline/Unlit")` 或默认 `Unlit/Color` 在 OpenXR **Single Pass Instanced** 下常只渲一只眼。不是深度/Queue 问题。

The shader has no VR stereo macros. `Shader.Find("Universal Render Pipeline/Unlit")` or default `Unlit/Color` often draws only one eye under OpenXR **Single Pass Instanced**. Depth/queue flags will not fix it.

**做法 / Do this：**

1. 用 `Vr3StereoUnlitShaderUtil.CreateMaterial(color)`（`VR3/Unlit Color Stereo`）。  
   Use `Vr3StereoUnlitShaderUtil.CreateMaterial(color)` (`VR3/Unlit Color Stereo`).
2. 自写 shader 须有 stereo `multi_compile`、`Cull Off`、frag 里 `UNITY_SETUP_STEREO_EYE_INDEX_POST_VERTEX`。  
   Custom shaders need stereo `multi_compile`, `Cull Off`, and `UNITY_SETUP_STEREO_EYE_INDEX_POST_VERTEX` in the fragment.
3. **禁止**指望 `ZTest Always` + 高 `renderQueue` 修单眼。  
   Do not try to fix one-eye missing with `ZTest Always` or a high `renderQueue`.

验收 / Verify: both eyes see the same colored cube.
