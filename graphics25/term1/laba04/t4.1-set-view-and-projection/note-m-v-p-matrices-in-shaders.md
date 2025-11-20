# Note: M V P matrices in shaders

In this lab, we still use the modelview matrix in  the vertex shader asfollows:

```glsl
// transformation in the world and camera space
uniform mat4 modelview;
uniform mat4 projection;
```

It is best to use separate model and view matrices in the vertex shader.

But no worry, we can change it in the next lab.
