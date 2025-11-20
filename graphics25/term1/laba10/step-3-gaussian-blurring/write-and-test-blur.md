---
description: main.cpp
---

# Write and Test blur()

### blur()

add one function blur() in main.cpp

First we need to set the uniform pass in bloomblur.frag.

Then we are going to perform horizontal and vertical blurring alternatively.

Finally we are going to draw the blurred image on a full-screen square, exported as a texture.

We are using blurtex\[0] and blurtex\[1] alternatively, in a ping-pong style.&#x20;

```cpp
void blur(int pass)
{
    GLuint loc = glGetUniformLocation(bloomblurShader, "pass" );
    glUniform1i(loc, pass);

    glActiveTexture(GL_TEXTURE2);

    if ((pass % 2) == 0)
    {
        // rending from texture 1 and writing to texture 2
        glBindTexture(GL_TEXTURE_2D, blurtex[0]);
        glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_TEXTURE_2D, blurtex[1], 0);          
    }
    else 
    {
        // reading from texture 2 and writing to texture 1
        glBindTexture(GL_TEXTURE_2D, blurtex[1]);
        glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_TEXTURE_2D, blurtex[0], 0);
    }
  
    // Render the full-screen quad
    square->setShaderId(bloomblurShader);
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

## call blur() in the event loop

We need to call blur() for a number of iterations, for example, 10 times or more.

```cpp
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();

        // Step 1
        render();
        
        // Step 2
        filter();
        
        // Step 3
        for (int i = 0; i < 50; i++)
        {
            blur(i);
        }
        
        glfwSwapBuffers(window);
    }
```

## Test blur()

We leave the test of blur() to the next step , until we write blend().
