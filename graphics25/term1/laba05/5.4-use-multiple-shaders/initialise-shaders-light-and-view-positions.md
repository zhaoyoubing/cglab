---
description: main.cpp
---

# Initialise Shaders, Light and View Positions

## setLightPosition()&#x20;

Add initLightPosition() after initShader()

```cpp
GLuint initShader(std::string pathVert, std::string pathFrag) 
{
    ...
}

void setLightPosition(glm::vec3 lightPos)
{
    GLuint lightpos_loc = glGetUniformLocation(shader.program, "lightPos" );
    glUniform3fv(lightpos_loc, 1, glm::value_ptr(lightPos));
}

void setViewPosition(glm::vec3 eyePos)
{
    GLuint viewpos_loc = glGetUniformLocation(shader.program, "viewPos" );
    glUniform3fv(viewpos_loc, 1, glm::value_ptr(eyePos));
}
```



As you are using glm::value\_ptr(), you need to add its header file in the headers list.

```cpp
#include <glm/gtc/type_ptr.hpp>
```



## Initialise shaders in main()

in main(), comment out the original initShader() call.

Add initialisation of  **Phong** and **Blinn-Phong** shader variables after loading GLAD.

As the light position uniform needs to be set for each shader, **setLightPosition**() is called after the initialisation of each shader.

```cpp
    // loading glad
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        ...
    }

    //initShader( "shaders/colour.vert", "shaders/colour.frag");

    phongShader = initShader( "shaders/blinn.vert", "shaders/phong.frag");
    setLightPosition(lightPos);
    setViewPosition(viewPos);
    blinnShader = initShader( "shaders/blinn.vert", "shaders/blinn.frag");
    setLightPosition(lightPos);
    setViewPosition(viewPos);
```
