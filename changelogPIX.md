# Branch -> Initial Inspections

total frame time: 19.82 ms


{PrePass DDM\_AllOpaqueNoVelocity (Forced by Nanite)} 394.53 μs



{RenderDeferredLighting} 10.21 ms

* {DiffuseIndirectAndAO}
*  	{LumenReflections}
*  		{ReflectionHardwareRayTracingCS default} 5.71 ms

|

* {Lights}
*  	{DirectLighting} 2.50 ms
*  		{VirtualShadowMapProjectionMaskBits} 1.44 ms

|

* {TranslucencyLightingVolume} 803.78 μs



{ComputeVolumetricFog} 1.01 ms



{PostProcessing}

L {TemporalSuperResolution(sg.AntiAliasingQuality=3) 1400x788 -> 1920x1080} 2.49 ms



Created Utility Blueprint to check if any mesh is enlisted for Nanite;

Disabled Nanite, since no high resolution meshes used in project there's no reason to use this feature and pay some cost in render pipeline;



Disabled Lumen RT Reflections, switched to Reflection Probes;



^ Commit d97cde4 {General RP Config}



Switched to Screen Space Reflections (SSR) + Reflection Probes as fallback;

Switched from Temporal Super Resolution (TSR) to Temporal Antialiasing (TAA) due to less jittering in low roughness materials, better visual fidelity;



^ Commit 9c555d8 {Visual Fidelity to Original and Performance}



## Final Results -> Initial Inspections



From starting 19.82 ms

to 13.06 ms



Inspect at "InitialInspections.wpix"



ToDo in the next Branch:

Solve:

* \[Graphics Shader]
* Base Pass 527 μs {reduce cost of Materials}
* Lights 1.49 ms {Too much light influence overlap? Get rid of Lumen? Shadows in useless areas?}
*  	BatchedLights 527 μs
*  	UnbatchedLights 964 μs
* DiffuseIndirectAndAO 5.16 ms
*  	DiffuseIndirectComposite(DiffuseIndirect=ScreenProbeGather) 1400x788 {Lumen issue}
* ComputeVolumetricFog 922.43 μs {Reduce voxel density? Reduce lights with volume scattering?}
* PostProcessing 947 μs
*  	TAA 238 μs {about 20% of PostProcess time}
*  	Bloom 153 μs
*  	Motion Blur 141 μs



---

# Branch -> Light Inspection



total frame time: ~13.06 ms



* RenderDeferredLighting 7,55 ms
* &nbsp;	DiffuseIndirectAndAO
* &nbsp;		DiffuseIndirectComposite(DiffuseIndirect=ScreenProbeGather) 1400x788 5,12 ms
* Lights 1,49 ms



Set temporal AA to 4 samples, seems to not make sense, but less samples in this project are helping in less shimmering in farther details and near puddles of water when transitioning from low to medium/high roughness;



start test(Reduce Lumen parameters in post process volume)

{

Lumen Scene Lighting Quality: 1 to 0.25 (No visual difference)

Lumen Scene Detail: 1 to 0.25 (No visual difference)

Lumen Scene View Distance: 20'000.0 to 1.0 (No visual difference)

Final Gather Quality, the main culpirit: 1.0 to 0.25 (No visual difference)

Max Trace Distance: 20'000.0 to 6'000.0 (Reduced until visual difference is imperceptible)

Scene Capture Cache Resolution Scale: 1 to 0.5 (No visual difference)



also tried the cvars:

* r.Lumen.ScreenProbeGather.IntegrateDownsampleFactor=2
* r.Lumen.ScreenProbeGather.RadianceCache.ProbeResolution=16
* r.Lumen.ScreenProbeGather.RadianceCache.NumProbesToTraceBudget=256
* r.Lumen.ScreenProbeGather.TracingOctahedronResolution=16
* r.Lumen.ScreenProbeGather.AdaptiveProbePlacement=1



in engine ~5ms

post compilation:

&nbsp;	DiffuseIndirectComposite(DiffuseIndirect=ScreenProbeGather) 1400x788 4,1 ms

} Lumen fail...



Disabled:

* Lumen;
* Mesh Distance Field;
* Any ray tracing related feature;



working on static lighting

