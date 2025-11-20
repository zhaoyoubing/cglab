---
description: Mesh.cpp main.cpp
---

# Use Different Shaders for Different Models

Congratulations!

Now you can use different shaders for different models when you initialise mesh models.



First double check that you have glUseProgram() in your Mesh::draw(), it should be there before.

#### check Mesh::draw()

check Mesh::draw() method, if you did not see glUseProgram() in the beginning of Mesh::draw(), you should add that

```cpp
void Mesh::draw(glm::mat4 matModel, glm::mat4 matView, glm::mat4 matProj)
     // 1. Bind the correct shader program
    glUseProgram(shaderId);
    ...
}
```



Now you only need to specify the shader when you call Mesh::init() in main()

For example:

```cpp
    //----------------------------------------------------
    // Meshes
    std::shared_ptr<Mesh> cube = std::make_shared<Mesh>();
    cube->init("models/cube.obj", blinnShader);

    std::shared_ptr<Mesh> teapot = std::make_shared<Mesh>();
    teapot->init("models/teapot.obj", phongShader);
```

Note: you should revise your existing code.
