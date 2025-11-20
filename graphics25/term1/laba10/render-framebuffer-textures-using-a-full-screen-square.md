# Render Framebuffer Textures using a Full Screen Square

### Create the Square

Here we use the factory pattern to create a full screen square to render the textures.

#### Add a public static method createSquare() in Mesh.h

```cpp
    // factory methods for creation of raw models
    static std::shared_ptr<Mesh> createSquare();
```

#### Define the method in Mesh.cpp

To  follow the format of a Vertex, we are manually adding vertex position, normal and texture coordinates. However, we only need the vertex position and texture coordinates for the full screen square. The normal vectors are only place holders.

```cpp
std::shared_ptr<Mesh> Mesh::createSquare()
 {
    std::shared_ptr<Mesh> obj = std::make_shared<Mesh>();
    obj->vertices = {
        {glm::vec3(-1.0f, -1.0f, 0.0f), 
         glm::vec3(0.0f, 0.0f, 1.0f),
         glm::vec2(0.0f, 0.0f)
        },
        {
         glm::vec3(1.0f, -1.0f, 0.0f), 
         glm::vec3(0.0f, 0.0f, 1.0f),
         glm::vec2(1.0f, 0.0f)
        },
        {
         glm::vec3(1.0f, 1.0f, 0.0f), 
         glm::vec3(0.0f, 0.0f, 1.0f),
         glm::vec2(1.0f, 1.0f)
        },
        {
         glm::vec3(-1.0f, 1.0f, 0.0f), 
         glm::vec3(0.0f, 0.0f, 1.0f),
         glm::vec2(0.0f, 1.0f)
        }
    };

    obj->indices = {
        0, 1, 2, 0, 2, 3
    };

    obj->initBuffer();

    return obj;
 }
```

#### Generate the square in main()

```cpp
square = Mesh::createSquare();
```

you need to call its draw() method during highlight filtering and Guassian blurring.
