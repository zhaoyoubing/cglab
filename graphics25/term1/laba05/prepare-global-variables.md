---
description: main.cpp
---

# Prepare Global Variables

In this section we are going to add variables needed for lighting in main.cpp.

First we are going to separate the model matrix and view matrix from the modelview matrix

Frist we are going to add the **light position** variable and the **camera position** variable.

```cpp
glm::vec3 lightPos = glm::vec3(5.0f, 5.0f, 10.0f);
glm::vec3 viewPos = glm::vec3(0.0f, 0.0f, 5.0f);
```

Because now you are using the global viewPos, you should remove redudant viewPos assignments before glLookAt() in main()

```cpp
   // set the eye at (0, 0, 5), looking at the centre of the world
    // try to change the eye position
    // viewPos = glm::vec3(0.0f, 0.0f, 5.0f);
    matView = glm::lookAt(viewPos, glm::vec3(0, 0, 0), glm::vec3(0, 1, 0)); 
```

Then we are going to add shader ids for flat shader, phong shader and blinn-phong shader

```cpp
GLuint blinnShader;
GLuint phongShader;
```

The whole list of global variables will be as follows

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
