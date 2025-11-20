# Update Mesh::initBuffer()

As we are now using struct Vertex, we need to revise Mesh::InitBuffer() to reflect that.

Moreover, we need to add the texture attribute.

## Revise the position and normal attributes

```cpp
    // changed in LabA07 
    // set buffer data to triangle vertex and setting vertex attributes
    glBufferData(GL_ARRAY_BUFFER, vertices.size() * sizeof(Vertex), &vertices[0] /*vertices.data()*/, GL_STATIC_DRAW);
    glEnableVertexAttribArray(0);
    //glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, sizeof(float) * 6, 0);
    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, sizeof(Vertex), 0);

    // set normal attributes
    glEnableVertexAttribArray(1);
    //glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, sizeof(float) * 6, (void *) (sizeof(float) * 3));
    glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, sizeof(Vertex), (void *) (sizeof(float) * 3));

```

## Adding the texture attribute

```cpp
    // added in LabA07: Adding texture coord attribute
    // vertex texture coords
    glEnableVertexAttribArray(2);
    // the second parameter: 2 coordinates (tx, ty) per texture coord	
    glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, sizeof(Vertex), (void*)offsetof(Vertex, texCoord));
```

## Full Source Code of Mesh::initBuffer()

```cpp
void Mesh::initBuffer()
{
    // create vertex buffer
    GLuint vao;
    glGenVertexArrays(1, &vao);
    GLuint vertBufID;
    glGenBuffers(1, &vertBufID);
    glBindBuffer(GL_ARRAY_BUFFER, vertBufID);
    GLuint idxBufID;
    glGenBuffers(1, &idxBufID);
    
    // remember VAO
    glBindVertexArray(vao);
    buffers.push_back(vao);
    
    std::cout << "vertBufId: " << vertBufID << std::endl;

    buffers.push_back(vertBufID);

    // changed in LabA07 
    // set buffer data to triangle vertex and setting vertex attributes
    glBufferData(GL_ARRAY_BUFFER, vertices.size() * sizeof(Vertex), &vertices[0] /*vertices.data()*/, GL_STATIC_DRAW);
    glEnableVertexAttribArray(0);
    //glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, sizeof(float) * 6, 0);
    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, sizeof(Vertex), 0);

    // set normal attributes
    glEnableVertexAttribArray(1);
    //glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, sizeof(float) * 6, (void *) (sizeof(float) * 3));
    glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, sizeof(Vertex), (void *) (sizeof(float) * 3));

    // added in LabA07: Adding texture coord attribute
    // vertex texture coords
    glEnableVertexAttribArray(2);
    // the second parameter: 2 coordinates (tx, ty) per texture coord	
    glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, sizeof(Vertex), (void*)offsetof(Vertex, texCoord));

    // bind index buffer
    glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, idxBufID);

    std::cout << "idxBufId: " << idxBufID << std::endl;
    buffers.push_back(idxBufID);

    // set buffer data for triangle index
    glBufferData(GL_ELEMENT_ARRAY_BUFFER, indices.size() * sizeof(GLuint), indices.data(), GL_STATIC_DRAW);

    glBindVertexArray(0);
}
```
