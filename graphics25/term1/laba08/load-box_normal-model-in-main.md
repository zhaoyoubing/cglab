---
description: main.cpp
---

# Load Box\_normal model in main()

### Define the normal map shader global variable

```cpp
GLuint blinnShader;
GLuint phongShader;
GLuint texblinnShader;

// LabA08 Normal map
GLuint normalmapShader;
```

### Init the normal map shader in main()

```cpp
    normalmapShader = initShader("shaders/normalmap.vert", "shaders/normalmap.frag");
    setLightPosition(lightPos);
    setViewPosition(viewPos);
```

### Load the Box\_normal model

```cpp
    std::shared_ptr<Mesh> box = std::make_shared<Mesh>();
    box->init("models/Box_normal.obj", normalmapShader);
```

### Draw the model

```cpp
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

        // for LabA08 Normal Map
        box->draw(matModelRoot, matView, matProj);
        
        glfwSwapBuffers(window);
    }
```



<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
