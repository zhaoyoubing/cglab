# Full Source Code of initRenderToTexture()

```cpp
void initRenderToTexture()
{
    // Generate and bind the framebuffer
    glGenFramebuffers(1, &colourFBO);
    glBindFramebuffer(GL_FRAMEBUFFER, colourFBO);

    // Create the texture object
    glGenTextures(1, &colourTex);
    glActiveTexture(GL_TEXTURE1);
    glBindTexture(GL_TEXTURE_2D, colourTex);
    glTexStorage2D(GL_TEXTURE_2D, 1, GL_RGB32F, width, height);

    // Bind the texture to the colour FBO
    glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_TEXTURE_2D, colourTex, 0);

    // Set the targets for the fragment output variables
    GLenum drawBuffers[] = {GL_COLOR_ATTACHMENT0};
    glDrawBuffers(1, drawBuffers);

    // Create the depth buffer
    GLuint depthBuf;
    glGenRenderbuffers(1, &depthBuf);
    glBindRenderbuffer(GL_RENDERBUFFER, depthBuf);
    glRenderbufferStorage(GL_RENDERBUFFER, GL_DEPTH_COMPONENT, width, height);

    // Bind the depth buffer to the FBO
    glFramebufferRenderbuffer(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT,
                              GL_RENDERBUFFER, depthBuf);

    // Create an FBO for the bright-pass filter and blur
    glGenFramebuffers(1, &blurFBO);
    glBindFramebuffer(GL_FRAMEBUFFER, blurFBO);

    // the bloom buffer should use a lower resolution to better support blurring 
    // such as width/4 and height/4 or more
    bloomBufWidth = width / 4;
    bloomBufHeight = height / 4;

    // Create two texture objects to ping-pong blur 
    glGenTextures(2, blurtex);
    glActiveTexture(GL_TEXTURE2);

    // blur texture 1
    glBindTexture(GL_TEXTURE_2D, blurtex[0]);
    glTexStorage2D(GL_TEXTURE_2D, 1, GL_RGB32F, bloomBufWidth, bloomBufHeight);

    // blur texture 2
    glBindTexture(GL_TEXTURE_2D, blurtex[1]);
    glTexStorage2D(GL_TEXTURE_2D, 1, GL_RGB32F, bloomBufWidth, bloomBufHeight);

    // Bind texture 1 to the FBO
    glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_TEXTURE_2D, blurtex[0], 0);

    // Unbind the framebuffer, and revert to default framebuffer
    glBindFramebuffer(GL_FRAMEBUFFER, 0);
}
```
