---
hidden: true
---

# Calculating Normals in JavaScript

### **JavaScript Tutorial: Calculating Triangle Normals and Vertex Normals for Triangle Meshes**

#### **1. Triangle Normals**

We'll start by calculating the normal for a triangle using the cross product of two edge vectors.

**1.1. Cross Product for Triangle Normals**

Given three vertices v1=(x1,y1,z1)v\_1 = (x\_1, y\_1, z\_1)v1​=(x1​,y1​,z1​), v2=(x2,y2,z2)v\_2 = (x\_2, y\_2, z\_2)v2​=(x2​,y2​,z2​), and v3=(x3,y3,z3)v\_3 = (x\_3, y\_3, z\_3)v3​=(x3​,y3​,z3​), we calculate the edge vectors:

* edge1=v2−v1\text{edge1} = v\_2 - v\_1edge1=v2​−v1​
* edge2=v3−v1\text{edge2} = v\_3 - v\_1edge2=v3​−v1​

The cross product of these two edge vectors gives the triangle normal.

#### **1.2. JavaScript Code for Triangle Normals**

```javascript
javascript复制编辑class Vec3 {
    constructor(x, y, z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    // Subtract two vectors
    subtract(v) {
        return new Vec3(this.x - v.x, this.y - v.y, this.z - v.z);
    }

    // Cross product of two vectors
    cross(v) {
        return new Vec3(
            this.y * v.z - this.z * v.y,
            this.z * v.x - this.x * v.z,
            this.x * v.y - this.y * v.x
        );
    }

    // Normalize the vector
    normalize() {
        const length = Math.sqrt(this.x * this.x + this.y * this.y + this.z * this.z);
        return new Vec3(this.x / length, this.y / length, this.z / length);
    }
}

// Function to calculate the normal of a triangle
function calculateTriangleNormal(v1, v2, v3) {
    const edge1 = v2.subtract(v1);
    const edge2 = v3.subtract(v1);
    const normal = edge1.cross(edge2);
    return normal.normalize();
}

// Example Usage
const v1 = new Vec3(0, 0, 0);
const v2 = new Vec3(1, 0, 0);
const v3 = new Vec3(0, 1, 0);

const normal = calculateTriangleNormal(v1, v2, v3);
console.log(`Normal: (${normal.x}, ${normal.y}, ${normal.z})`);
```

***

#### **2. Vertex Normals**

To calculate vertex normals, we will:

1. Iterate through each triangle in the mesh.
2. Calculate the normal of each triangle.
3. Add the triangle normal to the corresponding vertex normals.
4. Normalize the accumulated vertex normals.

#### **2.1. JavaScript Code for Vertex Normals**

```javascript
javascript复制编辑function calculateVertexNormals(vertices, indices) {
    // Initialize vertex normals to zero
    const vertexNormals = new Array(vertices.length).fill(null).map(() => new Vec3(0, 0, 0));

    // Iterate through the indices in groups of 3 (forming triangles)
    for (let i = 0; i < indices.length; i += 3) {
        const idx1 = indices[i];
        const idx2 = indices[i + 1];
        const idx3 = indices[i + 2];

        // Calculate the normal for the triangle
        const normal = calculateTriangleNormal(vertices[idx1], vertices[idx2], vertices[idx3]);

        // Add the normal to the vertices
        vertexNormals[idx1] = vertexNormals[idx1].add(normal);
        vertexNormals[idx2] = vertexNormals[idx2].add(normal);
        vertexNormals[idx3] = vertexNormals[idx3].add(normal);
    }

    // Normalize the vertex normals
    for (let i = 0; i < vertexNormals.length; i++) {
        vertexNormals[i] = vertexNormals[i].normalize();
    }

    return vertexNormals;
}

// Example Usage
const vertices = [
    new Vec3(0, 0, 0),
    new Vec3(1, 0, 0),
    new Vec3(0, 1, 0),
    new Vec3(0, 0, 1),
    new Vec3(1, 0, 1),
    new Vec3(0, 1, 1)
];

const indices = [0, 1, 2, 3, 4, 5];

const vertexNormals = calculateVertexNormals(vertices, indices);

// Print out the vertex normals
vertexNormals.forEach((normal, index) => {
    console.log(`Vertex ${index} Normal: (${normal.x}, ${normal.y}, ${normal.z})`);
});
```

***

#### **3. Explanation of the Code**

* **Vec3 Class**: The `Vec3` class defines a 3D vector with methods for subtraction, cross product, and normalization.
* **calculateTriangleNormal**: This function calculates the normal of a triangle by first creating two edge vectors from the triangle’s vertices, taking the cross product, and then normalizing the result.
* **calculateVertexNormals**: This function calculates the vertex normals by iterating over all the triangles and adding the triangle normals to the respective vertex normals. Afterward, it normalizes the vertex normals.

#### **4. Conclusion**

In this JavaScript tutorial, we've walked through how to calculate both triangle normals and vertex normals for a 3D mesh:

* **Triangle Normals**: Calculated using the cross product of two edge vectors.
* **Vertex Normals**: Calculated by averaging the triangle normals for all triangles that share a vertex.

These normals are crucial for proper lighting calculations, enabling smooth shading in 3D rendering. You can now use these normals in lighting models like **Phong shading** or **Blinn-Phong shading**.
