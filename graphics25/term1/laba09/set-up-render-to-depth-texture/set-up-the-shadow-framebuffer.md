---
description: main.cpp initRenderToDepthTexture()
---

# Set up the Shadow Framebuffer

Because we are going to render the depth offscreen, we need to generate the framebuffer object.

```cpp
    glGenFramebuffers(1, &shadowFBO);
    glBindFramebuffer(GL_FRAMEBUFFER, shadowFBO);
```

Then we specify to use the depth texture as the framebuffer

```cpp
    // use the depth texture as the framebuffer
    glFramebufferTexture2D(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT,
                           GL_TEXTURE_2D, depthTex, 0);
```

We don't need the colourbuffer, so we specify it as GL\_NONE

```cpp
glDrawBuffer(GL_NONE);
```

Finally, checking the framebuffer and restore the framebuffer to the default onscreen rendering

```cpp
    GLenum result = glCheckFramebufferStatus(GL_FRAMEBUFFER);
    if( result == GL_FRAMEBUFFER_COMPLETE) {
        printf("Framebuffer is complete.\n");
    } else {
        printf("Framebuffer is not complete.\n");
    }

    glBindFramebuffer(GL_FRAMEBUFFER,0);
```
