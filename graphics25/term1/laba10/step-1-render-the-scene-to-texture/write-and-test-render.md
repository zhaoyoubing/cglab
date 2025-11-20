---
description: main()
---

# Write and Test render()

Now we are going to use the shaders and render the scene.

## render()

add one render() function in main.cpp

```cpp
void render()
{
    glViewport(0,0,width,height);

    // comment glBindFramebuffer to test rendering
    glBindFramebuffer(GL_FRAMEBUFFER, colourFBO);
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glEnable(GL_DEPTH_TEST);
    
    scene->setShaderId(bloomrenderShader);
    scene->draw(matModelRoot, matView, matProj);
}
```

## init shaders in the shader section in main()

```cpp
    //==============================================================================
    // added for LabA10 Bloom
    bloomrenderShader = initShader("shaders/bloom.vert", "shaders/bloomrender.frag");
    setLightPosition(lightPos);
    setViewPosition(viewPos);
```

## call render() in the event loop

```cpp
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();

        render();
        
        glfwSwapBuffers(window);
    }
```

## Test render()

comment the line of glBindFrameBuffer() so that you can see the rendering in your window

```cpp
void render()
{
    glViewport(0,0,width,height);

    // comment glBindFramebuffer to test rendering
    // glBindFramebuffer(GL_FRAMEBUFFER, colourFBO);
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glEnable(GL_DEPTH_TEST);
    
    scene->setShaderId(bloomrenderShader);
    scene->draw(matModelRoot, matView, matProj);
}
```

You are going to see something similar to the following:

<figure><img src="../../../../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>
