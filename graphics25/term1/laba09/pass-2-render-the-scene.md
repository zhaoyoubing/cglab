---
description: main.cpp
---

# Pass 2: Render the Scene

## Set shader Uniforms

After setting the light space view and projection matrices, set matLightviewproj.

In addition, set shadowMap to texture unit 1.

```cpp
    glm::mat4 matLightviewproj = matLightProj * matLightView;
    GLuint loc = glGetUniformLocation(shadowShader, "lightSpaceMatrix" );
    glUniformMatrix4fv(loc, 1, GL_FALSE, &matLightviewproj[0][0]);
    GLuint textureLoc = glGetUniformLocation(shadowShader, "shadowMap");
    glUniform1i(textureLoc, 1);
```

## in the main event loop

Add the second rendering pass.

In addition, change the first rendering pass to offscreem rendering by binding the framebuffer to shadowFBO.

```cpp
while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();
        
        // 1st pass, draw the shadow depth map
        glBindFramebuffer(GL_FRAMEBUFFER, shadowFBO);
        
        ...

        
        // 2nd pass
        glBindFramebuffer(GL_FRAMEBUFFER, 0);
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        glViewport(0, 0, width, height);

        scene->setShaderId(shadowShader);
        scene->draw(matModelRoot, matView, matProj);
        

        glfwSwapBuffers(window);
    }
```

### Result

<figure><img src="../../../.gitbook/assets/image (8).png" alt="" width="563"><figcaption></figcaption></figure>
