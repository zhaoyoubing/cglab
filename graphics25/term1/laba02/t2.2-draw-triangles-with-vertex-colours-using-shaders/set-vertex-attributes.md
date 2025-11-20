---
description: Using glVertexAttribPointer
---

# Set vertex attributes

## glVertexAttribPointer() Function Signature

<table data-header-hidden><thead><tr><th width="337.3333740234375"></th><th></th></tr></thead><tbody><tr><td><code>void </code><strong><code>glVertexAttribPointer</code></strong><code>(</code></td><td>GLuint index,</td></tr><tr><td> </td><td>GLint size,</td></tr><tr><td> </td><td>GLenum type,</td></tr><tr><td> </td><td>GLboolean normalized,</td></tr><tr><td> </td><td>GLsizei stride, // vertex stride</td></tr><tr><td> </td><td>const void * pointer<code>)</code>; // attribute start pointer</td></tr></tbody></table>

## Modify vertex position attribute

now we have 6 floats for each vertex

```cpp
  glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, sizeof(float) * 6, 0);
```

## Add colour attribute

```cpp
    // set colour attributes
    glEnableVertexAttribArray(1);
    glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, sizeof(float) * 6, (void *) (sizeof(float) * 3));
```

We are still getting this

<figure><img src="../../../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

To render vertex colour we need to write shaders
