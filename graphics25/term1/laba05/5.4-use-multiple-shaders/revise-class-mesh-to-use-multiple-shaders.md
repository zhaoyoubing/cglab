---
description: Mesh.h and Mesh.cpp
hidden: true
---

# Revise class Mesh to use Multiple Shaders

## Add method setShaderId()

Because now we are uisng multiple shaders, so we need to tell the Mesh which shader it should use.

Add void setShaderId() in Mesh.h and Mesh.cpp

### Mesh.h

in the **public** section, add the following method declaration

```cpp
    void setShaderId(GLuint sid);
```

don't forget,  you should have defined shaderId earlier in the member list of Mesh

```cpp
class Mesh {
    protected:
    ....
    // this will be Material in the future
    GLuint shaderId;
    public:
    ...
    void setShaderId(GLuint sid);
    ...
};
```

### Mesh.cpp

In Mesh.cpp, we are adding a straightforward implmentation of  **setShaderId**()&#x20;

```cpp
void Mesh::setShaderId(GLuint sid) {
    shaderId = sid;
}
```

#### check Mesh::draw()

check Mesh::draw() method, if you did not see glUseProgram() in the beginning of Mesh::draw(), you should add that

```cpp
void Mesh::draw(glm::mat4 matModel, glm::mat4 matView, glm::mat4 matProj)
     // 1. Bind the correct shader program
    glUseProgram(shaderId);
    ...
}

```

