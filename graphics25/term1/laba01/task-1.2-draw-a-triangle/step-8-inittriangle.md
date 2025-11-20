# Step 8 initTriangle()

## Define function initTriangle()

##

In main.cpp, define a function initTriangle(), define the triangle:

```
void initTriangle()
{

}
```

## Add trangle data

Inside the function, adding

```cpp
GLfloat verts[] = {
    -1.0f, -1.0f,
    1.0f, -1.0f,
    0.0f, 1.0f,
};
```

## Create the vertex buffer

inside initTriangle(), adding code to create the buffer for holding the triangle vertex

```cpp
    GLuint bufID;
    glGenBuffers(1, &bufID);
    glBindBuffer(GL_ARRAY_BUFFER, bufID);
```

## Set vertex attributes

Set the buffer data to triangle vertex

Specify that each vetex has two coordinate components

```cpp
    glBufferData(GL_ARRAY_BUFFER, sizeof(verts), verts, GL_STATIC_DRAW);
    glEnableVertexAttribArray(0);
    glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, 0, 0);
```

