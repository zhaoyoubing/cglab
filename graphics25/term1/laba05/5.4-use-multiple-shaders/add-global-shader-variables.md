---
description: main.cpp
---

# Add Global Shader Variables

Add global variables for shader ids for Phong shader and Blinn-Phong shader

```cpp
GLuint blinnShader;
GLuint phongShader;
```

The whole list of global variables should be similar to the follows

```cpp
static Shader shader;

glm::mat4 matModelRoot = glm::mat4(1.0);
glm::mat4 matView = glm::mat4(1.0);
glm::mat4 matProj = glm::ortho(-2.0f,2.0f,-2.0f,2.0f, -2.0f,2.0f);

glm::vec3 lightPos = glm::vec3(5.0f, 10.0f, 20.0f);
glm::vec3 viewPos = glm::vec3(0.0f, 0.0f, 10.0f);

GLuint blinnShader;
GLuint phongShader;
```

