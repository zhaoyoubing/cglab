---
description: main.cpp
---

# Write and Test filter()

### filter()

add one function filter() in main.cpp

We are going to draw the full-screen square, using a texture from the scene rendering.

```cpp
void filter() 
{
    // unbind the framebuffer to test it in your window
    // glBindFramebuffer(GL_FRAMEBUFFER,0);
    glBindFramebuffer(GL_FRAMEBUFFER, blurFBO);

    // set the viewport size to bloom buffer width and height
    glViewport(0, 0, bloomBufWidth, bloomBufHeight);
    // depth test is not needed for image processing
    glDisable(GL_DEPTH_TEST);
    // clear the background
    glClearColor(0, 0, 0, 0);
    glClear(GL_COLOR_BUFFER_BIT);

    // draw the full screen square
    square->setShaderId(bloomfilterShader);
    square->draw(glm::mat4(1.0f), glm::mat4(1.0f), glm::mat4(1.0f));
}
```

## init shaders in the shader section in main()

We don't need to set eye and light positions because we don't need them for image processing.

```cpp
    //==============================================================================
    // added for LabA10 Bloom
    bloomrenderShader = ...;
    ...
    
    bloomfilterShader = initShader("shaders/bloom.vert", "shaders/bloomfilter.frag");
```

## call filter() in the event loop

```cpp
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();

        render();
        
        filter();
        
        glfwSwapBuffers(window);
    }
```

## Test filter()

Unbind the frame buffer to test result in your window.

```cpp
void filter() 
{
    // unbind the framebuffer to test it in your window
    glBindFramebuffer(GL_FRAMEBUFFER,0);
    //glBindFramebuffer(GL_FRAMEBUFFER, blurFBO);

    // set the viewport size to bloom buffer width and height
    glViewport(0, 0, bloomBufWidth, bloomBufHeight);
    // depth test is not needed for image processing
    glDisable(GL_DEPTH_TEST);
    // clear the background
    glClearColor(0, 0, 0, 0);
    glClear(GL_COLOR_BUFFER_BIT);

    // draw the full screen square
    square->setShaderId(bloomfilterShader);
    square->draw(glm::mat4(1.0f), glm::mat4(1.0f), glm::mat4(1.0f));
}
```

The result is similar to the following if you set the bloom buffer the same size as the window size. (Normally you should set it to a much lower resolution such as ¼ or 1/8) .

<figure><img src="../../../../.gitbook/assets/image (107).png" alt="" width="563"><figcaption></figcaption></figure>
