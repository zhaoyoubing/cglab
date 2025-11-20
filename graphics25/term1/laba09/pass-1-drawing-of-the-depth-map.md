---
description: main.cpp
---

# Pass 1: Drawing of the Depth Map

Now we are going to see what does the depth map look like.

### in the shader initialisation part of main(), create the depth shader

```cpp
blinnShader = initShader( "shaders/blinn.vert", "shaders/blinn.frag");
...

texblinnShader = initShader("shaders/texblinn.vert", "shaders/texblinn.frag");
...

// LabA09 Shadow Map
depthShader = initShader("shaders/simpledepth.vert", "shaders/simpledepth.frag");

...
```

### Create the view and projection matrices from the light source

We are using orthographic projections. You need to adjust the view volume, especially the near and far planes to make sure every objects in your scene are included.

```glsl
viewPos = ...
matView = ... 
matProj = ...

// LabA09 Shadow Map
matLightView = glm::lookAt(lightPos, glm::vec3(0, 0, 0), glm::vec3(0, 1, 0)); 
matLightProj = glm::ortho(-3.0f, 3.0f,-3.0f, 3.0f, -5.0f, 15.0f);

...
```

### Before the event loop, add a call to initRenderToDepthTexture()

```cpp
    glClearColor(0.25f, 0.5f, 0.75f, 1.0f);
    glEnable(GL_DEPTH_TEST);

    // LabA09 Shadow Map
    initRenderToDepthTexture();

    // setting the event loop
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();
        
        ...
    }
```

### In the event loop, replace the drawings of your model/scene

Note: the view and projection matrics to the **draw()** function are **matLightView** and **matLightProj**

```cpp
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();
        
        // 1st pass, draw the shadow depth map
        glClear(GL_DEPTH_BUFFER_BIT | GL_COLOR_BUFFER_BIT);

        glViewport(0,0,shadowMapWidth, shadowMapHeight);

        glEnable(GL_CULL_FACE);
        glCullFace(GL_FRONT);

        // you can also draw your meshes directly if you don't use a scene graph
        scene->setShaderId(depthShader);
        scene->draw(matModelRoot, matLightView, matLightProj);
        
        glCullFace(GL_BACK);
        glFlush();
        
         glfwSwapBuffers(window);
    }
```

### Result

You should see a depth map in gray. The following is an example, darker colours represent closer distance to the light source.

<figure><img src="../../../.gitbook/assets/image (9).png" alt="" width="563"><figcaption></figcaption></figure>
