---
description: Mesh.h
---

# Adding normal map support to Mesh

<figure><img src="../../../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

## Add bitangents in struct Vertex

```cpp
struct Vertex {
    glm::vec3 pos;
    glm::vec3 normal;
    glm::vec2 texCoord;
    
    // for normal mapping,  added in LabA08
    // tangent
    glm::vec3 tangent;
    // bitangent
    glm::vec3 bitangent;
};
```

## Add normal map list in Mesh

But we only support one mormal map per mesh.

```cpp
class Mesh {

protected:
    ...

    std::vector<Texture> textures;

    // added in LabA08 
    std::vector<Texture> normals;

    std::vector<GLuint> buffers;

    // this will be Material in the future
    GLuint shaderId;
    
    ...
};
```
