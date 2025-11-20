---
description: main.cpp
---

# Global Variables

### Shadow Map Size

```cpp
int shadowMapWidth = 1000;
int shadowMapHeight = 1000;
```

### Window Viewport Size

```cpp
int width = 800;
int height = 800;
```

### Depth Texture ID and Shadow Map Framebuffer ID

```cpp
GLuint depthTex;  // depth texture ID
GLuint shadowFBO; // shadow frame buffer ID
```

### Depth Shader ID and Shadow Rendering Shader ID

```cpp
GLuint depthShader;  // depth shader program ID
GLuint shadowShader; // shadow map shader program ID
```

### View and Projection Matrices from the Light Source

```cpp
glm::mat4 matLightView; // view matrix from the light source
glm::mat4 matLightProj; // projection matrix from the light source
```
