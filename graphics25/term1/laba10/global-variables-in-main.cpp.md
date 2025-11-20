# Global Variables in main.cpp

In main.cpp we are defining several variables needed for the bloom effect

### Shader Ids

```cpp
GLuint bloomrenderShader;
GLuint bloomfilterShader;
GLuint bloomblurShader;
GLuint bloomblendShader;
```

### Window size and bloom framebuffer size

```cpp
int width = 800;
int height = 800;
int bloomBufWidth;
int bloomBufHeight;
```

### Framebuffer ID and texture ID

```cpp
// framebuffer ID
GLuint colourFBO, blurFBO;
// render texture ID and blurring buffer ID
GLuint colourTex, blurtex[2];
```

### The square mesh for rendering full screen texture

```cpp
std::shared_ptr<Mesh> square;
```

### In addition, we are going to move the scene as a global variable

#### With Scene Graph

```
std::shared_ptr<Node> scene;
```

#### Without Scene Graph, e.g. use the teapot directly

```
std::shared_ptr<Mesh> teapot;
```
