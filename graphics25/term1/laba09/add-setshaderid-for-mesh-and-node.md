# Add setShaderId() for Mesh and Node

## Mesh

### Mesh.h

```cpp
class Mesh {
...
public:
    ...
    void setShaderId(GLuint sid);
    ...
};
```

### Mesh.cpp

```cpp
void Mesh::setShaderId(GLuint sid) {
    shaderId = sid;
}
```

## Node

If you are using a scene graph, then you also need to add setShaderId() in class Node

### Node.h

```cpp
class Node {
...
public:
    ...
    void setShaderId(GLuint sid);
    ...
};
```

### Node.cpp

```cpp
void Node::setShaderId(GLuint sid) {
    for (auto & m : meshes) {
        m->setShaderId(sid);
    }

    for (auto & n : childNodes) {
        n->setShaderId(sid);
    }
}
```

