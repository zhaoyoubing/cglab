---
hidden: true
---

# Vertex Shader

\#version 330 core layout(location = 0) in vec3 inPosition; layout(location = 1) in vec3 inNormal; layout(location = 2) in vec2 inTexCoord;

uniform mat4 model; uniform mat4 view; uniform mat4 proj;

out vec3 vWorldPos; out vec3 vNormal; out vec2 vUV;

void main() { vec4 worldPos = model \* vec4(inPosition, 1.0); vWorldPos = worldPos.xyz; vNormal = mat3(transpose(inverse(model))) \* inNormal; vUV = inTexCoord; gl\_Position = proj \* view \* worldPos; }
