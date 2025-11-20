# Set up the scene framebuffer and texture

We are going to render the scene to a texture first.

### Generate the framebuffer&#x20;

```
    // Generate and bind the framebuffer
    glGenFramebuffers(1, &colourFBO);
    glBindFramebuffer(GL_FRAMEBUFFER, colourFBO);
```

### Generate the texture id for the render target

```cpp
    // Create the texture object
    glGenTextures(1, &colourTex);
    glActiveTexture(GL_TEXTURE1);
    glBindTexture(GL_TEXTURE_2D, colourTex);
    glTexStorage2D(GL_TEXTURE_2D, 1, GL_RGB32F, width, height);

    // Bind the texture to the colour FBO
    glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, 
                           GL_TEXTURE_2D, colourTex, 0);
   
     // Set the targets for the fragment output variables
    GLenum drawBuffers[] = {GL_COLOR_ATTACHMENT0};
    glDrawBuffers(1, drawBuffers);
```

### Set up the depth buffer as well

```cpp
   // Create the depth buffer
    GLuint depthBuf;
    glGenRenderbuffers(1, &depthBuf);
    glBindRenderbuffer(GL_RENDERBUFFER, depthBuf);
    glRenderbufferStorage(GL_RENDERBUFFER, GL_DEPTH_COMPONENT, width, height);

    // Bind the depth buffer to the FBO
    glFramebufferRenderbuffer(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT,
                              GL_RENDERBUFFER, depthBuf);
```

