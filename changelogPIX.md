# Initial Inspections

total frame time: 19.82 ms
{PrePass DDM\_AllOpaqueNoVelocity (Forced by Nanite)} 394.53 μs



{RenderDeferredLighting} 10.21 ms

L {DiffuseIndirectAndAO}

| L {LumenReflections}

|   L {ReflectionHardwareRayTracingCS default} 5.71 ms

|

L {Lights}

| L {DirectLighting} 2.50 ms

|   L {VirtualShadowMapProjectionMaskBits} 1.44 ms

|

L {TranslucencyLightingVolume} 803.78 μs



{ComputeVolumetricFog} 1.01 ms



{PostProcessing}

L {TemporalSuperResolution(sg.AntiAliasingQuality=3) 1400x788 -> 1920x1080} 2.49 ms



Created Utility Blueprint to check if any mesh is enlisted for Nanite;

Disabled Nanite, since no high resolution meshes used in project there's no reason to use this feature and pay some cost in render pipeline;



Disabled Lumen RT Reflections, switched to Reflection Probes;



^ Commit 





## Final Results -> Initial Inspections



From starting 19.82 ms

to 16.55 ms



Look at 

