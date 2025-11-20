# Calculating Normals in C++

### **Calculating Triangle Normals and Vertex Normals for Triangle Meshes**

#### **1. Triangle Normals**

We'll calculate the normal of a triangle using the cross product of two edge vectors, just like in the JavaScript version.

#### **1.1. C++ Code for Triangle Normals**

```cpp
cpp复制编辑#include <iostream>
#include <vector>
#include <cmath>

struct Vec3 {
    float x, y, z;

    // Constructor
    Vec3(float x, float y, float z) : x(x), y(y), z(z) {}

    // Subtract two vectors
    Vec3 subtract(const Vec3& v) const {
        return Vec3(x - v.x, y - v.y, z - v.z);
    }

    // Cross product of two vectors
    Vec3 cross(const Vec3& v) const {
        return Vec3(
            y * v.z - z * v.y,
            z * v.x - x * v.z,
            x * v.y - y * v.x
        );
    }

    // Normalize the vector
    Vec3 normalize() const {
        float length = std::sqrt(x * x + y * y + z * z);
        return Vec3(x / length, y / length, z / length);
    }

    // Add another vector to this vector
    void add(const Vec3& v) {
        x += v.x;
        y += v.y;
        z += v.z;
    }
};

// Function to calculate the normal of a triangle
Vec3 calculateTriangleNormal(const Vec3& v1, const Vec3& v2, const Vec3& v3) {
    Vec3 edge1 = v2.subtract(v1);
    Vec3 edge2 = v3.subtract(v1);
    Vec3 normal = edge1.cross(edge2);
    return normal.normalize();
}

// Example Usage
int main() {
    Vec3 v1(0.0f, 0.0f, 0.0f);
    Vec3 v2(1.0f, 0.0f, 0.0f);
    Vec3 v3(0.0f, 1.0f, 0.0f);

    Vec3 normal = calculateTriangleNormal(v1, v2, v3);
    std::cout << "Normal: (" << normal.x << ", " << normal.y << ", " << normal.z << ")" << std::endl;
    return 0;
}
```

***

#### **2. Vertex Normals**

For vertex normals, we will:

1. Iterate through each triangle in the mesh.
2. Calculate the normal of each triangle.
3. Add the triangle normal to the corresponding vertex normals.
4. Normalize the accumulated vertex normals.

#### **2.1. C++ Code for Vertex Normals**

```cpp
cpp复制编辑// Function to calculate vertex normals
void calculateVertexNormals(const std::vector<Vec3>& vertices, const std::vector<int>& indices, std::vector<Vec3>& vertexNormals) {
    // Initialize vertex normals to zero
    vertexNormals.resize(vertices.size(), Vec3(0.0f, 0.0f, 0.0f));

    // Iterate through the indices in groups of 3 (forming triangles)
    for (size_t i = 0; i < indices.size(); i += 3) {
        int idx1 = indices[i];
        int idx2 = indices[i + 1];
        int idx3 = indices[i + 2];

        // Calculate the normal for the triangle
        Vec3 normal = calculateTriangleNormal(vertices[idx1], vertices[idx2], vertices[idx3]);

        // Add the normal to the vertices
        vertexNormals[idx1].add(normal);
        vertexNormals[idx2].add(normal);
        vertexNormals[idx3].add(normal);
    }

    // Normalize the vertex normals
    for (size_t i = 0; i < vertexNormals.size(); ++i) {
        vertexNormals[i] = vertexNormals[i].normalize();
    }
}

// Example Usage
int main() {
    // Vertices of a mesh
    std::vector<Vec3> vertices = {
        Vec3(0.0f, 0.0f, 0.0f),
        Vec3(1.0f, 0.0f, 0.0f),
        Vec3(0.0f, 1.0f, 0.0f),
        Vec3(0.0f, 0.0f, 1.0f),
        Vec3(1.0f, 0.0f, 1.0f),
        Vec3(0.0f, 1.0f, 1.0f)
    };

    // Indices of the triangles (each group of 3 indices forms a triangle)
    std::vector<int> indices = {0, 1, 2, 3, 4, 5};

    std::vector<Vec3> vertexNormals;
    calculateVertexNormals(vertices, indices, vertexNormals);

    // Print out the vertex normals
    for (size_t i = 0; i < vertexNormals.size(); ++i) {
        std::cout << "Vertex " << i << " Normal: (" 
                  << vertexNormals[i].x << ", " 
                  << vertexNormals[i].y << ", " 
                  << vertexNormals[i].z << ")" << std::endl;
    }

    return 0;
}
```

***

#### **3. Explanation of the Code**

* **Vec3 Class**: This structure represents a 3D vector and has methods for vector operations such as subtraction, cross product, normalization, and addition.
* **calculateTriangleNormal**: This function computes the normal of a triangle by first creating two edge vectors from the triangle’s vertices, performing the cross product, and normalizing the result.
* **calculateVertexNormals**: This function computes the vertex normals by iterating over all the triangles, calculating the normal for each triangle, and adding the normal to the corresponding vertex normals. After all normals are accumulated, they are normalized.

#### **4. Conclusion**

This C++ tutorial demonstrates how to calculate triangle normals and vertex normals for a 3D mesh:

* **Triangle Normals**: Calculated by taking the cross product of two edge vectors.
* **Vertex Normals**: Calculated by averaging the triangle normals for all triangles that share the vertex.

These normals are essential for proper lighting and shading in 3D rendering. Once you have the normals, you can use them in lighting models, such as **Phong shading** or **Blinn-Phong shading**, to achieve realistic rendering effects.
