# Write blend()

### blend()

In the blend step, we are going to draw directly on the screen instead of render-to-texture.

```cpp
void blend()
{
    // Bind to the default framebuffer to draw on the screen
    glBindFramebuffer(GL_FRAMEBUFFER,0);

    glClear(GL_COLOR_BUFFER_BIT );
    glViewport(0,0,width,height);

    // Render the full-screen quad
    square->setShaderId(bloomblendShader);
    square->draw(glm::mat4(1.0f), glm::mat4(1.0f), glm::mat4(1.0f));    
}
```

## init shaders in the shader section in main()

We don't need to set eye and light positions because we don't need them for image processing.

```cpp
    //==============================================================================
    // added for LabA10 Bloom
    bloomrenderShader = initShader("shaders/bloom.vert", "shaders/bloomrender.frag");
    setLightPosition(lightPos);
    setViewPosition(viewPos);

    bloomfilterShader = initShader("shaders/bloom.vert", "shaders/bloomfilter.frag");

    bloomblurShader = initShader("shaders/bloom.vert", "shaders/bloomblur.frag");

    bloomblendShader = initShader("shaders/bloom.vert", "shaders/bloomblend.frag");
    //==============================================================================
```

## call blend() in the event loop

```cpp
while (!glfwWindowShouldClose(window))
{
        glfwPollEvents();

        // Step 1
        render();
        
        // Step 2
        filter();

        // Step 3
        for (int i = 0; i < 10; i++)
        {
            blur(i);
        }

        // Step 4
        blend();
        
        //scene->draw(matModelRoot, matView, matProj);
        //bunny->draw(glm::scale(glm::vec3(0.005f, 0.005f, 0.005f)), matView, matProj);
        
        glfwSwapBuffers(window);
}
```

