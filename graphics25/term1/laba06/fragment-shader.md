---
hidden: true
---

# Fragment Shader

```
#version 330 core
in vec3 vWorldPos;
in vec3 vNormal;
in vec2 vUV;

out vec4 fragColor;

uniform vec3 camPos;            // camera world position
uniform vec3 lightPos;          // point light position
uniform vec3 lightColor;        // point light colour (linear)
uniform float lightIntensity;   // light intensity / distance scaling

// Material (PBR) inputs
uniform sampler2D albedoMap;    // albedo (srgb ideally; we'll linearize if needed)
uniform sampler2D normalMap;    // normal map (optional)
uniform sampler2D metallicMap;  // metallic (0..1)
uniform sampler2D roughnessMap; // roughness (0..1)
uniform sampler2D aoMap;        // ambient occlusion

// Constants
const float PI = 3.14159265359;

// ----- Helper functions -----

vec3 getNormalFromMap(vec3 n, vec2 uv) {
    // If you don't use a normal map, just return normalized input normal.
    // If using a normal map, replace this with proper tangent-space normal mapping.
    return normalize(n);
}

// Schlick Fresnel approximation
vec3 fresnelSchlick(float cosTheta, vec3 F0) {
    // Schlick's approximation
    return F0 + (1.0 - F0) * pow(1.0 - cosTheta, 5.0);
}

// GGX / Trowbridge-Reitz normal distribution function
float DistributionGGX(vec3 N, vec3 H, float roughness) {
    float a = roughness * roughness;
    float a2 = a * a;
    float NdotH = max(dot(N, H), 0.0);
    float NdotH2 = NdotH * NdotH;

    float denom = (NdotH2 * (a2 - 1.0) + 1.0);
    denom = PI * denom * denom;
    return a2 / max(denom, 1e-8);
}

// Geometry term: Schlick-GGX for a single direction
float GeometrySchlickGGX(float NdotV, float roughness) {
    float r = (roughness + 1.0);
    float k = (r * r) / 8.0; // Epic Games remapping for energy conservation
    return NdotV / (NdotV * (1.0 - k) + k);
}

// Smith's geometry (both directions)
float GeometrySmith(vec3 N, vec3 V, vec3 L, float roughness) {
    float NdotV = max(dot(N, V), 0.0);
    float NdotL = max(dot(N, L), 0.0);
    float ggxV = GeometrySchlickGGX(NdotV, roughness);
    float ggxL = GeometrySchlickGGX(NdotL, roughness);
    return ggxV * ggxL;
}

// Cook-Torrance BRDF - returns specular term
vec3 CookTorranceBRDF(vec3 N, vec3 V, vec3 L, vec3 F0, float roughness) {
    vec3 H = normalize(V + L);
    float NDF = DistributionGGX(N, H, roughness);
    float G   = GeometrySmith(N, V, L, roughness);
    float NdotL = max(dot(N, L), 0.0);
    float NdotV = max(dot(N, V), 0.0);
    float HdotV = max(dot(H, V), 0.0);

    vec3 F = fresnelSchlick(HdotV, F0);

    vec3 numerator = NDF * G * F;
    float denom = 4.0 * NdotV * NdotL + 1e-8;
    return numerator / denom;
}

void main() {
    // Sample material maps
    vec3 albedo = pow(texture(albedoMap, vUV).rgb, vec3(2.2)); // assume sRGB texture -> linear
    float metallic = texture(metallicMap, vUV).r;
    float roughness = clamp(texture(roughnessMap, vUV).r, 0.05, 1.0);
    float ao = texture(aoMap, vUV).r;

    // Geometry & directions
    vec3 N = normalize(vNormal);
    vec3 V = normalize(camPos - vWorldPos);
    vec3 L = normalize(lightPos - vWorldPos);
    vec3 H = normalize(V + L);

    // Compute reflectance at normal incidence (F0)
    vec3 F0 = vec3(0.04);                // dielectric base reflectance
    F0 = mix(F0, albedo, metallic);     // metals use albedo as F0

    // --- Cook-Torrance specular ---
    vec3 specular = CookTorranceBRDF(N, V, L, F0, roughness);

    // --- kD diffuse term (Lambert) ---
    vec3 kS = fresnelSchlick(max(dot(H, V), 0.0), F0); // specular factor
    vec3 kD = vec3(1.0) - kS;
    kD *= 1.0 - metallic; // non-metals have diffuse component

    // Light attenuation (simple inverse-square)
    float distance = length(lightPos - vWorldPos);
    float attenuation = lightIntensity / (distance * distance);
    vec3 radiance = lightColor * attenuation;

    float NdotL = max(dot(N, L), 0.0);
    vec3 Lo = (kD * albedo / PI + specular) * radiance * NdotL;

    // Ambient / AO (very simple)
    vec3 ambient = vec3(0.03) * albedo * ao;

    vec3 color = ambient + Lo;

    // HDR tonemapping (ACES approximation) and gamma correct
    // simple filmic tonemapping (uncharted)
    color = color / (color + vec3(1.0));
    color = pow(color, vec3(1.0 / 2.2));

    fragColor = vec4(color, 1.0);
}
```

* The shader above assumes tangent-space normal mapping is _not_ used; `getNormalFromMap` is a placeholder. If you have a normal map, transform it from tangent space to world space using the TBN matrix.
* `roughness` is clamped to a small minimum (0.05) to avoid extreme specular peaks and numerical issues.
* `fresnelSchlick` here uses Schlick’s 5th-power approximation; for grazing improvements you can use a roughness-aware variant (`F = F0 + (max(vec3(1.0 - roughness), F0) - F0) * pow(1.0 - cosTheta, 5.0)`), often seen in production shaders.
* For realistic lighting add IBL: sample prefiltered environment maps for specular, and an irradiance map for diffuse. Also replace the single point-light loop with multiple light sources as required.
