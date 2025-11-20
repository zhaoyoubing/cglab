# Ambient Occlusion

| **Method** | **Description** | **Used in** | **Pros** | **Cons** |
| ---------- | --------------- | ----------- | -------- | -------- |

| **Precomputed AO (baked)** | AO is precomputed offline in texture maps (AO maps) during asset baking in tools like Blender, Substance Painter, or Unreal’s Lightmass. | Static geometry | High quality, cheap at runtime | Not dynamic; doesn’t respond to moving objects or lighting |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ------------------------------ | ---------------------------------------------------------- |

| **SSAO (Screen-Space Ambient Occlusion)** | AO estimated from the depth buffer and normals of the rendered frame. | Nearly all modern engines | Real-time; works with dynamic scenes | View-dependent; can’t occlude off-screen geometry; noisy |
| ----------------------------------------- | --------------------------------------------------------------------- | ------------------------- | ------------------------------------ | -------------------------------------------------------- |

| **HBAO / HBAO+ (Horizon-Based AO)** | Improved SSAO by sampling the “horizon” angle of occlusion. Developed by NVIDIA. | Unreal, Unity HDRP, Frostbite | Better quality and stability | Slightly more expensive than SSAO |
| ----------------------------------- | -------------------------------------------------------------------------------- | ----------------------------- | ---------------------------- | --------------------------------- |

| **GTAO (Ground-Truth AO)** | Approximation of ray-traced AO using analytic integration over the horizon; balances quality and cost. | Unreal Engine 5, modern titles | Very high fidelity, efficient | Still view-dependent |
| -------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------ | ----------------------------- | -------------------- |

| **RTAO (Ray-Traced AO)** | True AO computed with real ray tracing (DXR, Vulkan RT). | Unreal, Unity HDRP, proprietary engines | Physically correct, dynamic | Requires hardware RT support, costly on performance |
| ------------------------ | -------------------------------------------------------- | --------------------------------------- | --------------------------- | --------------------------------------------------- |
