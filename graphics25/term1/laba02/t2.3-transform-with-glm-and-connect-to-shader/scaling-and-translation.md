# Scaling and translation

We are starting from the easist scale transform

## include glm header&#x20;

in main.cpp, add the header files of glm.hpp

```cpp
#define GLM_ENABLE_EXPERIMENTAL
#include <glm/glm.hpp>
#include <glm/gtc/matrix_transform.hpp>
#include <glm/gtx/transform.hpp>
```



## Generate the scale matrix

Here we are scaling the triangles by a factor of 0.5

```cpp
    glm::mat4 mat_scale = glm::scale(  // Scale first
        glm::mat4(1.0),              // identity matrix
        glm::vec3( 0.5f, 0.5f, 0.5f )
    );
```



## Change colour.vert

in the vertex shader colour.vert, declare a uniform for model transformation

```glsl
// transformation in the world and camera space
uniform mat4 modelview;
```

Transoform the vertex position using the matrix

```glsl
void main()
{
    gl_Position = modelview * vec4(pos, 1.0); 
    colour_vert = colour_in;
}
```

## share variable with GPU using **uniform** &#x20;

In initTriangle() in main.cpp, connect it with the modelview matrix created by GLM

```cpp
    glm::mat4 mat_modelview = mat_scale;
    
    GLuint modelview_loc = glGetUniformLocation( shader.program, "modelview" );
    glUniformMatrix4fv(modelview_loc, 1, GL_FALSE, &mat_modelview[0][0]);
```

Now we get

<figure><img src="../../../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

## Translation

We can also translate the scaled model, for example 0.3 on X and 0.2 on Y

```cpp
    glm::mat4 mat_scale = glm::scale(glm::vec3(0.5f, 0.5f, 0.5f));
    glm::mat4 mat_trans = glm::translate(glm::vec3(0.3f, 0.2f, 0.0f));

    glm::mat4 mat_modelview = mat_trans * mat_scale;
```



<figure><img src="../../../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

