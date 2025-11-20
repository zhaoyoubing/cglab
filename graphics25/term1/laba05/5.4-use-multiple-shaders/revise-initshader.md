---
description: main.cpp
---

# Revise initShader()

Because we are going to use multiple shaders, we like initShader() to return the shader id. Accordingly, we are changing the return value of initShader() from **void** to **GLuint**.

```cpp
GLuint initShader(std::string pathVert, std::string pathFrag) 
{
    shader.read_source( pathVert.c_str(), pathFrag.c_str());

    shader.compile();
    glUseProgram(shader.program);

    return shader.program;
}
```

