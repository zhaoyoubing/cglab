---
description: main.cpp
---

# Load our shader program

## Read shader files

We are going to use Shader::read\_source to read our vertex and fragment shader program in main()

```cpp
    // Initialize shader with VSCode
    shader.read_source( "../shaders/colour.vert", "../shaders/colour.frag");
    // use the following if you are using Visual Studio
    // shader.read_source( "shaders/colour.vert", "shaders/colour.frag");
    shader.compile();
    glUseProgram(shader.program);
```

Note: from LabA03, with the updated CMakeLists.txt, we will have uniform access of the shader files for both VSCode and Visuali Studio :

```cpp
shader.read_source( "shaders/colour.vert", "shaders/colour.frag");
```

## Finally, we get

<figure><img src="../../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>
