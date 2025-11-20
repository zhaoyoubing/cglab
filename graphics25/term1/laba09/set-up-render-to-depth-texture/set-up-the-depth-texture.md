---
description: main.cpp
---

# Set up the Depth Texture

### Generate the Depth Texture ID

```cpp
    // Generate the depth texture ID
    glGenTextures(1, &depthTex);
    glBindTexture(GL_TEXTURE_2D, depthTex);
```

### Set Depth Texture Format and Size

```cpp
    // set texture size and format, high resolution 24bit for depth
    glTexStorage2D(GL_TEXTURE_2D, 1, GL_DEPTH_COMPONENT24, shadowMapWidth, shadowMapHeight);
```

### Set Interpolation and Out-of-range parameters

```cpp
    // set texture parameters
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT); 
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);  
```

### Bind the Depth Texture to Texture Unit 1

```cpp
    // Assign the depth buffer texture to texture channel 1
    glActiveTexture(GL_TEXTURE1);
    glBindTexture(GL_TEXTURE_2D, depthTex);

```
