# Scriptable Render Pipeline
`2022-03-14`

A summary of how Unity's Scriptable Render Pipeline compares to the built-in render pipeline. This includes [URP](https://docs.unity3d.com/Manual/universal-render-pipeline.html), [HDRP](https://docs.unity3d.com/Manual/high-definition-render-pipeline.html), and custom [SRP](https://docs.unity3d.com/Manual/ScriptableRenderPipeline.html)s.

## Graphics performance and profiling
For the built-in render pipeline [optimizing draw calls](https://docs.unity3d.com/Manual/optimizing-draw-calls.html) used to be a big part of improving rendering performance.

The [Rendering Statistics panel](https://docs.unity3d.com/Manual/RenderingStatistics.html) gives a quick overview of how many draw calls were batched together.

![Rendering statistics](https://docs.unity3d.com/uploads/Main/GameViewStats.png)

The [Rendering Profiler module](https://docs.unity3d.com/Manual/ProfilerRendering.html) shows a breakdown of watch got batched why.

![Rendering profiler](https://docs.unity3d.com/uploads/Main/RenderProfiler.png)

## Profiling



## SRP Batcher
https://docs.unity3d.com/Manual/SRPBatcher.html
https://docs.unity3d.com/Manual/FrameDebugger.html
https://docs.unity3d.com/Manual/RenderingStatistics.html
https://docs.unity3d.com/Manual/ProfilerRendering.html


## GPU instancing
https://docs.unity3d.com/Manual/GPUInstancing.html
https://docs.unity3d.com/Manual/optimizing-draw-calls.html

## Notes
- Rendering profiler doesn't show SRP batches
- Stats panel doesn't show SRP batches.
- Frame debugger shows SRP batches.
- `MaterialPropertyBlock` break SRP batcher.
- SRP batcher is not as fast as GPU instancing but close and way more flexible.
- SRP batches across material instances if the shader variant is the same
- a shader variant consists of a shader and its keywords
- `URP/Lit` optimizes away unused passes based on the textures assigned. this creates variants.
