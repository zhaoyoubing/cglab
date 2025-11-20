---
description: Loading OBJ model with vertices, faces and normals.
---

# The OBJ Mesh Format

#### Loading OBJ 3D Mesh Models in C++

**1. 3D Model File Format**

3D models are often stored in specific file formats that describe the geometry of the model and, optionally, other information such as materials, textures, and transformations. Some common 3D model file formats include:

* **OBJ**: A simple and widely-used text-based format that describes 3D geometry (vertices, faces, and normals).
* **FBX**: A binary format used by Autodesk's software, supporting complex 3D animations and scenes.
* **STL**: A simple format used primarily for 3D printing, containing only the geometry (vertices and faces).
* **GLTF**: A JSON-based format often used for real-time rendering and web applications, often accompanied by binary data for geometry and textures.

The **OBJ** format is one of the simplest and most commonly used formats for 3D models. It's a text file format that represents the vertices, faces, and other geometry-related data for 3D models.

***

**2. OBJ Model Format**

The OBJ file format consists of the following elements:

* **Vertices (`v`)**: Defines the 3D coordinates of a point in space. A line starting with "v" defines a vertex.
* **Normals (`vn`)**: Defines the direction of a surface at a vertex. A line starting with "vn" defines a normal vector.
* **Texture Coordinates (`vt`)**: Defines the 2D coordinates for mapping textures onto the 3D surface. A line starting with "vt" defines a texture coordinate.
* **Faces (`f`)**: Defines how vertices are connected to form triangles or polygons. A line starting with "f" defines a face.

Here’s an example of a simple OBJ file:

```
nginx复制编辑v 0.0 0.0 0.0
v 1.0 0.0 0.0
v 0.0 1.0 0.0
f 1 2 3
```

In the example:

* `v 0.0 0.0 0.0` defines the first vertex at the origin.
* `v 1.0 0.0 0.0` defines a second vertex at `(1, 0, 0)`.
* `v 0.0 1.0 0.0` defines a third vertex at `(0, 1, 0)`.
* `f 1 2 3` defines a face using the first, second, and third vertices.

You can extend this format with normals and texture coordinates, and faces can reference these additional elements.

***

**3. A Simple OBJ Model Reader in C++**

Below is a simple OBJ model reader implemented in C++. This program will read the vertices and faces from an OBJ file and store them in appropriate data structures.

**C++ Code: OBJ Model Reader**

```cpp
cpp复制编辑#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>
#include <sstream>

struct Vertex {
    float x, y, z;
};

struct Face {
    int v1, v2, v3;  // Indices of the vertices making up the triangle
};

class OBJModel {
public:
    std::vector<Vertex> vertices;
    std::vector<Face> faces;

    bool load(const std::string& filename) {
        std::ifstream file(filename);
        if (!file.is_open()) {
            std::cerr << "Failed to open file: " << filename << std::endl;
            return false;
        }

        std::string line;
        while (std::getline(file, line)) {
            std::istringstream ss(line);
            std::string prefix;
            ss >> prefix;

            if (prefix == "v") { // Vertex data
                Vertex v;
                ss >> v.x >> v.y >> v.z;
                vertices.push_back(v);
            }
            else if (prefix == "f") { // Face data
                Face f;
                ss >> f.v1 >> f.v2 >> f.v3;
                faces.push_back(f);
            }
        }

        file.close();
        return true;
    }

    void print() {
        std::cout << "Vertices:" << std::endl;
        for (const auto& v : vertices) {
            std::cout << v.x << " " << v.y << " " << v.z << std::endl;
        }

        std::cout << "\nFaces:" << std::endl;
        for (const auto& f : faces) {
            std::cout << f.v1 << " " << f.v2 << " " << f.v3 << std::endl;
        }
    }
};

int main() {
    OBJModel model;
    if (model.load("model.obj")) {
        model.print();
    }
    return 0;
}
```

#### Explanation of the Code:

1. **Vertex and Face Structures**:
   * `Vertex`: Represents a 3D point with `x`, `y`, and `z` coordinates.
   * `Face`: Represents a triangle face by referencing 3 vertices.
2. **OBJModel Class**:
   * `vertices`: A vector of `Vertex` objects that store all the vertices in the OBJ model.
   * `faces`: A vector of `Face` objects that store all the faces in the OBJ model.
   * `load()` method: Opens the specified OBJ file, reads the vertices and faces, and stores them in the respective vectors.
   * `print()` method: Prints the loaded vertices and faces to the console for verification.
3. **Loading the OBJ File**:
   * The program opens the OBJ file, reads it line-by-line, and parses it based on the prefix (`v` for vertices and `f` for faces).
   * The vertex data is stored as `x, y, z` coordinates, and face data is stored as indices (1-based) referencing the vertices.
