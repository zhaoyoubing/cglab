# Use vertex index

in initTriangle, making following changes

## Vertex and index definition

There are now no duplicate vertices

```cpp
    // two triangles : vertex data
    GLfloat verts[] = {
        -1.0f, 1.0f,  0.0f,// v0
        -1.0f, -1.0f, 0.0f,// v1
        1.0f, -1.0f,  0.0f, // v2
        1.0f, 1.0f,   0.0f,// v3
    };

    // indices of two triangles
    GLuint indices[] = { 0, 1, 2, 2, 3, 0};
```

We are moving from 2d vetex to 3d, so there is some change

### The Vertex Position Attribute

The vertex position attribute is similar to that in LabA01, the difference is that now we are using 3d vertices.

```cpp
    // set buffer data to triangle vertex and setting vertex attributes
    glBufferData(GL_ARRAY_BUFFER, sizeof(verts), verts, GL_STATIC_DRAW);
    glEnableVertexAttribArray(0);
    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 0, 0);
```

### The Vertex Indices

After the code of vertex buffer, create triangle index buffer of type GL\_ELEMENT\_ARRAY\_BUFFER

```cpp
    // create index buffer
    GLuint idxBufID;
    glGenBuffers(1, &idxBufID);
    glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, idxBufID);

    // set buffer data for triangle index
    glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```

## Draw triangles with indices

in drawTriangle, making following changes

```cpp
    // glDrawArrays(GL_TRIANGLES, 0, 6);
    glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

Now you get a rectangle

<figure><img src="../../../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

If you are using line drawing in drawTriangle(), you can see the two triangles

```cpp
glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);
```

