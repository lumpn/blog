# Profiling the Scriptable Render Pipeline
`2022-03-14`

A summary of how to profile Unity's Scriptable Render Pipeline. This includes [URP](https://docs.unity3d.com/Manual/universal-render-pipeline.html), [HDRP](https://docs.unity3d.com/Manual/high-definition-render-pipeline.html), and custom [SRP](https://docs.unity3d.com/Manual/ScriptableRenderPipeline.html)s.

## TL;DR:
1. Forget about GPU instancing.
2. Forget about `MaterialPropertyBlock`s.
3. Careful with unintentional shader variants.

## Profiling the Built-in Render Pipeline
Back in the old days Unity only had a single render pipeline, now called the [Built-in Render Pipeline](https://docs.unity3d.com/Manual/built-in-render-pipeline.html) (BiRP). Profiling what the BiRP is doing is easy enough.

### Stats
![Rendering statistics](https://docs.unity3d.com/uploads/Main/GameViewStats.png)

The [Rendering Statistics panel](https://docs.unity3d.com/Manual/RenderingStatistics.html) gives a quick overview of useful statistics like triangles and vertices rendered as well as, how many draw calls were batched together.

### Rendering Profiler
![Rendering profiler](https://docs.unity3d.com/uploads/Main/RenderProfiler.png)

The [Rendering Profiler module](https://docs.unity3d.com/Manual/ProfilerRendering.html) shows a breakdown of watch got batched and why.

### Batching
Besides obvious indicators like triangles and vertices, a lot of profiler work goes into batching. Why? It turns out that [draw calls are expensive](https://docs.unity3d.com/Manual/optimizing-draw-calls.html), so optimizing rendering performance often involves reducing draw calls, e.g. through [GPU instancing](https://docs.unity3d.com/Manual/GPUInstancing.html), [static batching](https://docs.unity3d.com/Manual/static-batching.html), or [dynamic batching](https://docs.unity3d.com/Manual/dynamic-batching.html).

The reason why there are so many different methods addressing the same problem is that none of them are universally applicable.
- GPU instancing only works for the same mesh.
- Static batching only works for static geometry that doesn't move.
- Dynamic batching produces so much overhead that it's almost never useful.

Oh and all of them only work if it's the same material, i.e. literally the same material instance. That's why we get [MaterialPropertyBlock](https://docs.unity3d.com/ScriptReference/MaterialPropertyBlock.html) to change material properties without breaking draw call batching. You might even go so far to require a single material across your entire project, using only `MaterialPropertyBlock`s at runtime to assign textures.

## Profiling the Scriptable Render pipeline
The first thing to note when profiling the Scriptable Render Pipeline is that the rendering profiler and the statistics panel are broken.

### GPU instancing
Using SRP, GPU instancing does not work. Try as you might, the number of batched draw calls due to instancing is always zero. That is because [SRP comes with its own batcher](https://docs.unity3d.com/Manual/SRPBatcher.html) and Unity prioritizes that SRP batcher over GPU instancing.

| Render pipeline | Foo |
-------------------------
| bar | baz |

### Static batching
Static batching works


## Graphics performance and profiling
For the built-in render pipeline [optimizing draw calls](https://docs.unity3d.com/Manual/optimizing-draw-calls.html) used to be a big part of improving rendering performance.





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
