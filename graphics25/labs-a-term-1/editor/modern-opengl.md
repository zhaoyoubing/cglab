---
description: Drawing Triangles with OpenGL, GLEW, and GLFW
---

# Modern OpenGL

##

### Objective

This tutorial guides you through rendering triangles using OpenGL with GLEW and GLFW. We start by drawing a single triangle, then expand to a triangle soup, and finally construct a small indexed triangle mesh.

### Prerequisites

* C++ development environment (e.g., Visual Studio, Code::Blocks, or GCC)
* OpenGL, GLEW, and GLFW installed

### OpenGL Evolution

&#x20;[OpenGL 1.1](https://www.khronos.org/opengl/wiki/History_of_OpenGL#OpenGL_1.1_.281997.29) (1997) : addition of **vertex arrays**, which allows for the number of GL calls to specify a point, line or polygon, which could easily get into double figures, to drop to one (not including some setup calls).

[OpenGL 1.5](https://www.khronos.org/opengl/wiki/History_of_OpenGL#OpenGL_1.5_.282003.29) (2003) : addition of **vertex buffer objects**, which builts on the vertex array specification and which allowed for data that didn't need to change to be kept in GPU memory.

***

### Step 1: Setting Up the Environment

1. Install GLFW and GLEW if not already installed.
2. Create a new C++ project and set up OpenGL.
3. Include necessary headers:

```cpp
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>
```

4. Initialize GLFW and create a window:

```cpp
if (!glfwInit()) {
    std::cerr << "Failed to initialize GLFW" << std::endl;
    return -1;
}
GLFWwindow* window = glfwCreateWindow(800, 600, "OpenGL Triangle", nullptr, nullptr);
if (!window) {
    std::cerr << "Failed to create GLFW window" << std::endl;
    glfwTerminate();
    return -1;
}
glfwMakeContextCurrent(window);
```

5. Initialize GLEW:

```cpp
glewExperimental = GL_TRUE;
if (glewInit() != GLEW_OK) {
    std::cerr << "Failed to initialize GLEW" << std::endl;
    return -1;
}
```

***

### Step 2: Drawing a Single Triangle

1. Define a triangle:

```cilkcpp
float vertices[] = {
    0.0f,  0.5f, 0.0f,
   -0.5f, -0.5f, 0.0f,
    0.5f, -0.5f, 0.0f
};
```

2. Create and bind a Vertex Buffer Object (VBO) and Vertex Array Object (VAO):

```cpp
GLuint VBO, VAO;
glGenVertexArrays(1, &VAO);
glGenBuffers(1, &VBO);
glBindVertexArray(VAO);
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
glBindBuffer(GL_ARRAY_BUFFER, 0);
glBindVertexArray(0);
```

3. Create a simple inlined vertex and fragment shader.

```glsl
const char* vertexShaderSource = "#version 330 core\n"
    "layout (location = 0) in vec3 aPos;\n"
    "void main()\n"
    "{\n"
    "   gl_Position = vec4(aPos, 1.0);\n"
    "}\0";

const char* fragmentShaderSource = "#version 330 core\n"
    "out vec4 FragColor;\n"
    "void main()\n"
    "{\n"
    "   FragColor = vec4(1.0, 0.0, 0.0, 1.0);\n"
    "}\n\0";
```

3. Inside the render loop:

<pre class="language-cpp"><code class="lang-cpp"><strong>glClear(GL_COLOR_BUFFER_BIT);
</strong>glUseProgram(shaderProgram);
glBindVertexArray(VAO);
glDrawArrays(GL_TRIANGLES, 0, 3);
glBindVertexArray(0);
</code></pre>

***

### Step 3: Drawing Multiple Raw Triangles

1. Define multiple triangles:

```cpp
float vertices[] = {
    -0.5f,  0.5f, 0.0f,
     0.0f, -0.5f, 0.0f,
     0.5f,  0.5f, 0.0f,

    -0.75f, -0.5f, 0.0f,
    -0.25f, -0.5f, 0.0f,
    -0.5f,   0.0f, 0.0f
};
```

2. Adjust `glDrawArrays` to render multiple triangles:

```
glDrawArrays(GL_TRIANGLES, 0, 6);
```

***

### Step 4: Drawing a Triangle Mesh with Indices

1. Define vertices and indices:

```cpp
float vertices[] = {
    -0.5f, -0.5f, 0.0f,
     0.5f, -0.5f, 0.0f,
     0.5f,  0.5f, 0.0f,
    -0.5f,  0.5f, 0.0f
};

unsigned int indices[] = {
    0, 1, 2,
    2, 3, 0
};
```

2. Create and bind an Element Buffer Object (EBO):

```cpp
GLuint EBO;
glGenBuffers(1, &EBO);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```

3. Modify the draw call:

```cpp
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

`glDrawArrays: raw triangles with a fixed` indexing pattern, one of `GL_TRIANGLES`, `GL_TRIANGLE_STRIP` or `GL_TRIANGLE_FAN`.&#x20;

glDrawElements:  using index buffer.  With `glDrawElements` you can specify a mixed indexing pattern within the single draw call.  However, you have to generate, store and load that index buffer onto the GPU, which adds extra time and takes extra space.

## Drawing Indexed Triangles with Vertex Colours

### Adding Colour Attribute to Vertices

```cpp
// defining vertices
float vertices[] = {
    // (x, y, z)   (R, G, B)
    0.0,  0.0, 0.0, 1.0, 0.0, 0.0,
    1.0,  0.0, 0.0, 0.0, 1.0, 0.0,
    1.0,  1.0, 0.0, 0.0, 0.0, 1.0,
    0.0,  1.0, 0.0, 1.0, 1.0, 0.0
};
```

### Setting Data Buffer Layout

Now each vertex has its own colour attribute. We use glVertexAttribPointer to tell the shader program this data layout.

For glVertexAttribPointer, its second parameter specifies the size of the attribute, its 5th parameter specifies the stride (number of floats for each vertex), and its last parameter specifies the pointer to the first element.

```cpp
void glVertexAttribPointer(GLuint index, GLint size, GLenum type, 
           GLboolean normalized, GLsizei stride, const void * offset);
/*          
index: attribute index, corresponds to location in the vertex shader, 
       e.g. vertex location normally comes with 0
size: number of components, e.g. 3 for a 3d vertex
type: data type, e.g. GL_FLOAT  
normalized: false
stride: size of each vertex data, in byte.
        e.g. for a 3d vertex with (r, g, b) value, is 6 * sizeof(float) 
offset: start offset of the attribute in each row, in bytes. 
        This is sometimes confusing.
*/
```

For our data, the stride is 6 \* sizeof(float). The offset is 0 for vertex position attribute, 3 \* sizeof(float) for the colour attribute . The following code sets the vertex location and vertex colour attributes using index retrieved from the vertex shader.

```cpp
int posLoc = glGetAttribLocation(shaderProgram, "aPos");
glVertexAttribPointer(posLoc, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)0);
glEnableVertexAttribArray(posLoc);
int posCol = glGetAttribLocation(shaderProgram, "aColour");
glVertexAttribPointer(posCol, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*) (3 * sizeof(float)));
glEnableVertexAttribArray(posCol);
```

The drawing code in the loop needs no change.

### Conclusion

This tutorial demonstrated how to render a single triangle, multiple triangles, and an indexed triangle mesh using OpenGL with GLEW and GLFW. Next steps could involve adding colors, textures, or interactivity.
