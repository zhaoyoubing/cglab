# Normal Calculation

### **Calculating Triangle Normals and Vertex Normals for Triangle Meshes**

When working with 3D meshes, the normals are used to represent how the surface of a mesh interacts with light. Each triangle face in a mesh has a normal, and these normals can be averaged to calculate the normals at each vertex.

In this tutorial, we will:

1. **Calculate the face normal for each triangle** in a mesh.
2. **Calculate vertex normals** by averaging the face normals of adjacent triangles.

***

#### **1. Triangle Normals**

A **triangle normal** is a vector that is perpendicular to the surface of the triangle. The normal vector can be calculated using the **cross product** of two edge vectors of the triangle.

**1.1. Cross Product for Triangle Normals**

Given three vertices v1=(x1,y1,z1)v\_1 = (x\_1, y\_1, z\_1)v1​=(x1​,y1​,z1​), v2=(x2,y2,z2)v\_2 = (x\_2, y\_2, z\_2)v2​=(x2​,y2​,z2​), and v3=(x3,y3,z3)v\_3 = (x\_3, y\_3, z\_3)v3​=(x3​,y3​,z3​) of a triangle, we can calculate two edge vectors:

* edge1=v2−v1\text{edge1} = v\_2 - v\_1edge1=v2​−v1​
* edge2=v3−v1\text{edge2} = v\_3 - v\_1edge2=v3​−v1​

The cross product of these two edge vectors gives the triangle normal:

N=edge1×edge2\mathbf{N} = \mathbf{edge1} \times \mathbf{edge2}N=edge1×edge2

In component form, the cross product is:

N=(edge1y⋅edge2z−edge1z⋅edge2y,edge1z⋅edge2x−edge1x⋅edge2z,edge1x⋅edge2y−edge1y⋅edge2x)\mathbf{N} = \left( \text{edge1}\_y \cdot \text{edge2}\_z - \text{edge1}\_z \cdot \text{edge2}\_y, \text{edge1}\_z \cdot \text{edge2}\_x - \text{edge1}\_x \cdot \text{edge2}\_z, \text{edge1}\_x \cdot \text{edge2}\_y - \text{edge1}\_y \cdot \text{edge2}\_x \right)N=(edge1y​⋅edge2z​−edge1z​⋅edge2y​,edge1z​⋅edge2x​−edge1x​⋅edge2z​,edge1x​⋅edge2y​−edge1y​⋅edge2x​)

**1.2. Normalizing the Triangle Normal**

To make sure the normal is a unit vector (magnitude of 1), we normalize the result:

N=N∥N∥\mathbf{N} = \frac{\mathbf{N\}}{\\| \mathbf{N} \\|}N=∥N∥N​

Where ∥N∥\\| \mathbf{N} \\|∥N∥ is the magnitude (length) of the vector N\mathbf{N}N.

**1.3. Example Code for Calculating Triangle Normals**

Here’s a simple example of how to calculate the normal of a triangle in C++:

```cpp
#include <iostream>
#include <cmath>

struct Vec3 {
    float x, y, z;

    Vec3 operator-(const Vec3& v) const {
        return Vec3{x - v.x, y - v.y, z - v.z};
    }

    // Cross product of two vectors
    Vec3 cross(const Vec3& v) const {
        return Vec3{
            y * v.z - z * v.y,
            z * v.x - x * v.z,
            x * v.y - y * v.x
        };
    }

    // Normalize the vector
    Vec3 normalize() const {
        float length = std::sqrt(x * x + y * y + z * z);
        return Vec3{x / length, y / length, z / length};
    }
};

Vec3 calculateTriangleNormal(const Vec3& v1, const Vec3& v2, const Vec3& v3) {
    Vec3 edge1 = v2 - v1;
    Vec3 edge2 = v3 - v1;
    Vec3 normal = edge1.cross(edge2);
    return normal.normalize();
}

int main() {
    Vec3 v1 = {0.0f, 0.0f, 0.0f};
    Vec3 v2 = {1.0f, 0.0f, 0.0f};
    Vec3 v3 = {0.0f, 1.0f, 0.0f};

    Vec3 normal = calculateTriangleNormal(v1, v2, v3);

    std::cout << "Normal: (" << normal.x << ", " << normal.y << ", " << normal.z << ")\n";
    return 0;
}
```

***

#### **2. Vertex Normals**

A **vertex normal** is the average of the normals of all the triangles that share that vertex. By averaging the face normals, we get a smooth transition between adjacent faces.

**2.1. Steps to Calculate Vertex Normals**

1. For each triangle in the mesh, calculate the triangle normal.
2. For each vertex, accumulate the normals of all the triangles that share that vertex.
3. Normalize the accumulated normal to ensure it’s a unit vector.

**2.2. Example Code for Calculating Vertex Normals**

Assume you have a mesh represented as a list of vertices and indices for the faces. The vertex normals are calculated by iterating over the faces and adding the face normals to the corresponding vertex normals.

```cpp
#include <iostream>
#include <vector>

struct Vec3 {
    float x, y, z;

    Vec3 operator-(const Vec3& v) const {
        return Vec3{x - v.x, y - v.y, z - v.z};
    }

    // Cross product
    Vec3 cross(const Vec3& v) const {
        return Vec3{
            y * v.z - z * v.y,
            z * v.x - x * v.z,
            x * v.y - y * v.x
        };
    }

    // Normalize the vector
    Vec3 normalize() const {
        float length = std::sqrt(x * x + y * y + z * z);
        return Vec3{x / length, y / length, z / length};
    }

    // Add another vector to this one
    void add(const Vec3& v) {
        x += v.x;
        y += v.y;
        z += v.z;
    }
};

// Function to calculate the normal of a triangle
Vec3 calculateTriangleNormal(const Vec3& v1, const Vec3& v2, const Vec3& v3) {
    Vec3 edge1 = v2 - v1;
    Vec3 edge2 = v3 - v1;
    return edge1.cross(edge2).normalize();
}

// Function to calculate vertex normals
void calculateVertexNormals(const std::vector<Vec3>& vertices, const std::vector<int>& indices, std::vector<Vec3>& vertexNormals) {
    // Initialize vertex normals to zero
    vertexNormals.resize(vertices.size(), Vec3{0.0f, 0.0f, 0.0f});

    // Calculate normals for each triangle and add them to vertex normals
    for (size_t i = 0; i < indices.size(); i += 3) {
        int idx1 = indices[i];
        int idx2 = indices[i + 1];
        int idx3 = indices[i + 2];

        Vec3 normal = calculateTriangleNormal(vertices[idx1], vertices[idx2], vertices[idx3]);

        // Add this normal to each vertex that makes up the triangle
        vertexNormals[idx1].add(normal);
        vertexNormals[idx2].add(normal);
        vertexNormals[idx3].add(normal);
    }

    // Normalize all vertex normals
    for (Vec3& normal : vertexNormals) {
        normal = normal.normalize();
    }
}

int main() {
    // Vertices of a mesh
    std::vector<Vec3> vertices = {
        {0.0f, 0.0f, 0.0f}, {1.0f, 0.0f, 0.0f}, {0.0f, 1.0f, 0.0f},
        {0.0f, 0.0f, 1.0f}, {1.0f, 0.0f, 1.0f}, {0.0f, 1.0f, 1.0f}
    };

    // Indices of the triangles
    std::vector<int> indices = {0, 1, 2, 3, 4, 5};

    std::vector<Vec3> vertexNormals;
    calculateVertexNormals(vertices, indices, vertexNormals);

    // Print out the normals for each vertex
    for (const Vec3& normal : vertexNormals) {
        std::cout << "Normal: (" << normal.x << ", " << normal.y << ", " << normal.z << ")\n";
    }

    return 0;
}
```

***

#### **3. Explanation of the Code**

* **Vec3 Struct**: A simple structure representing a 3D vector with methods for vector operations such as subtraction, cross product, and normalization.
* **Triangle Normal Calculation**: The function `calculateTriangleNormal` computes the normal for each triangle using the cross product of its edges.
* **Vertex Normal Calculation**: The function `calculateVertexNormals` iterates through each triangle, calculates its normal, and adds it to the normals of the vertices that make up the triangle. After all normals are accumulated, they are normalized.

#### **4. Conclusion**

In this tutorial, we’ve covered how to calculate both triangle normals and vertex normals for a 3D mesh:

* **Triangle Normals**: We used the cross product of two edge vectors to compute the normal of a triangle face.
* **Vertex Normals**: We averaged the triangle normals for all the adjacent triangles that share each vertex to smooth out the lighting effects.

By calculating the normals for your mesh, you enable proper lighting calculations, which is essential for rendering 3D scenes. You can now use these normals for lighting calculations like **Phong shading** or **Blinn-Phong shading** in your rendering pipeline.
