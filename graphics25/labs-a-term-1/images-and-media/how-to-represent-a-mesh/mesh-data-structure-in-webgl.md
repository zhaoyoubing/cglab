# Mesh Data Structure in WebGL

### **Data Structures for Representing a Mesh Model and Calculating Normals in WebGL**

In WebGL, the core concept of representing a mesh is the same as in C++, but we use JavaScript and the WebGL API for rendering and calculations.

We will cover:

* Data structures to represent vertices, normals, and faces in WebGL.
* How to calculate face and vertex normals for a triangle mesh.
* A complete WebGL program that loads vertices and normals and renders a simple triangle mesh.

***

#### **1. Data Structures for Representing a Mesh in WebGL**

**1.1. Vertex Structure**

A vertex structure typically contains the position of the vertex (x, y, z) and can optionally include texture coordinates (u, v) and normals (nx, ny, nz). In WebGL, we will use JavaScript arrays to store these attributes.

```javascript
javascript复制编辑// Vertex data structure
const vertices = [
    // Positions           // Normals
    0.0,  0.0,  0.0,       0.0,  0.0,  1.0,   // Vertex 1
    1.0,  0.0,  0.0,       0.0,  0.0,  1.0,   // Vertex 2
    0.0,  1.0,  0.0,       0.0,  0.0,  1.0    // Vertex 3
];
```

In this array, every 6 consecutive values represent a vertex with its position and normal. This simple representation can be used for a basic triangle mesh.

**1.2. Indexing for Faces (Elements)**

In WebGL, we use an index array to refer to the vertices that make up each face (usually triangles). This reduces the amount of data needed to define the mesh.

```javascript
javascript复制编辑const indices = [
    0, 1, 2  // Triangle face formed by vertices 0, 1, 2
];
```

Each set of three consecutive values in the `indices` array defines a face in the mesh.

**1.3. Mesh Representation in WebGL**

In WebGL, we don’t explicitly define a `Mesh` class like in C++. Instead, we use JavaScript arrays for storing vertex positions, normals, and indices, and pass them to WebGL buffers.

***

#### **2. Calculating Normals for Triangle Meshes in WebGL**

In WebGL, normals can be computed in two ways:

* **Per-face normals**: These are normals calculated for each triangle face.
* **Per-vertex normals**: These are averaged normals from all the faces that share a vertex.

**2.1. Per-Face Normal Calculation**

The normal for a triangle face can be calculated by taking the cross product of two edges of the triangle. The two edges are formed by subtracting the positions of the triangle’s vertices.

The steps:

1. Compute two edge vectors by subtracting two vertices from a third vertex.
2. Compute the cross product of these two edge vectors.
3. Normalize the resulting vector to get the face normal.

In WebGL, this can be done in JavaScript as follows:

```javascript
javascript复制编辑function calculateFaceNormal(v1, v2, v3) {
    const edge1 = [v2[0] - v1[0], v2[1] - v1[1], v2[2] - v1[2]];
    const edge2 = [v3[0] - v1[0], v3[1] - v1[1], v3[2] - v1[2]];

    const normal = [
        edge1[1] * edge2[2] - edge1[2] * edge2[1], // Nx
        edge1[2] * edge2[0] - edge1[0] * edge2[2], // Ny
        edge1[0] * edge2[1] - edge1[1] * edge2[0]  // Nz
    ];

    // Normalize the normal
    const length = Math.sqrt(normal[0] ** 2 + normal[1] ** 2 + normal[2] ** 2);
    return [normal[0] / length, normal[1] / length, normal[2] / length];
}
```

**2.2. Per-Vertex Normal Calculation**

To compute per-vertex normals, we need to accumulate the normals of all adjacent faces that share each vertex. For each vertex:

1. Add the normals of all faces sharing this vertex.
2. Normalize the resulting normal vector.

```javascript
javascript复制编辑function calculateVertexNormals(vertices, indices) {
    const vertexNormals = new Array(vertices.length).fill([0, 0, 0]);

    for (let i = 0; i < indices.length; i += 3) {
        const v1 = vertices[indices[i] * 6];      // position of vertex 1
        const v2 = vertices[indices[i + 1] * 6];  // position of vertex 2
        const v3 = vertices[indices[i + 2] * 6];  // position of vertex 3

        const faceNormal = calculateFaceNormal(v1, v2, v3);

        // Add the face normal to each vertex normal
        for (let j = 0; j < 3; j++) {
            const idx = indices[i + j];
            vertexNormals[idx] = vertexNormals[idx].map((val, idx2) => val + faceNormal[idx2]);
        }
    }

    // Normalize the vertex normals
    return vertexNormals.map(normal => {
        const length = Math.sqrt(normal[0] ** 2 + normal[1] ** 2 + normal[2] ** 2);
        return [normal[0] / length, normal[1] / length, normal[2] / length];
    });
}
```

**2.3. Combining with WebGL Buffers**

Once we have the normals calculated, we can upload the vertex data and normal data to WebGL buffers for rendering. We use **attribute buffers** to store the vertices and normals, and **element buffers** to store the face indices.

***

#### **3. WebGL Example: Rendering a Mesh with Calculated Normals**

Here’s how you can combine the calculations into a WebGL program that loads a triangle mesh, calculates per-vertex normals, and renders it to the screen.

**Complete WebGL Program**

```html
html复制编辑<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WebGL Mesh with Normals</title>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { width: 100%; height: 100%; display: block; }
    </style>
</head>
<body>
<canvas id="webgl-canvas"></canvas>
<script>
const canvas = document.getElementById('webgl-canvas');
const gl = canvas.getContext('webgl');
if (!gl) {
    alert("WebGL not supported");
}

// Define the vertices (positions and normals)
const vertices = [
    // Position       // Normal
    0.0, 0.0, 0.0,    0.0, 0.0, 1.0,
    1.0, 0.0, 0.0,    0.0, 0.0, 1.0,
    0.0, 1.0, 0.0,    0.0, 0.0, 1.0
];

const indices = [
    0, 1, 2
];

// Create buffers
const vertexBuffer = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer);
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(vertices), gl.STATIC_DRAW);

const indexBuffer = gl.createBuffer();
gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, indexBuffer);
gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, new Uint16Array(indices), gl.STATIC_DRAW);

// Vertex and fragment shaders
const vertexShaderSource = `
    attribute vec3 a_position;
    attribute vec3 a_normal;
    varying vec3 v_normal;
    void main() {
        gl_Position = vec4(a_position, 1.0);
        v_normal = a_normal;
    }
`;

const fragmentShaderSource = `
    precision mediump float;
    varying vec3 v_normal;
    void main() {
        float light = dot(normalize(v_normal), vec3(0.0, 0.0, 1.0));
        gl_FragColor = vec4(vec3(1.0, 0.5, 0.0) * light, 1.0);
    }
`;

// Compile shaders
const vertexShader = gl.createShader(gl.VERTEX_SHADER);
gl.shaderSource(vertexShader, vertexShaderSource);
gl.compileShader(vertexShader);

const fragmentShader = gl.createShader(gl.FRAGMENT_SHADER);
gl.shaderSource(fragmentShader, fragmentShaderSource);
gl.compileShader(fragmentShader);

// Link shaders into program
const program = gl.createProgram();
gl.attachShader(program, vertexShader);
gl.attachShader(program, fragmentShader);
gl.linkProgram(program);
gl.useProgram(program);

// Bind vertex buffer to attribute
const positionLocation = gl.getAttribLocation(program, "a_position");
gl.vertexAttribPointer(positionLocation, 3, gl.FLOAT, false, 6 * 4, 0);
gl.enableVertexAttribArray(positionLocation);

// Bind normal buffer to attribute
const normalLocation = gl.getAttribLocation(program, "a_normal");
gl.vertexAttribPointer(normalLocation, 3, gl.FLOAT, false, 6 * 4, 3 * 4);
gl.enableVertexAttribArray(normalLocation);

// Set the canvas size
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// Clear the screen and render
gl.clearColor(0.0, 0.0, 0.0, 1.0);
gl.clear(gl.COLOR_BUFFER_BIT);

gl.drawElements(gl.TRIANGLES, indices.length, gl.UNSIGNED_SHORT, 0);
</script>
</body>
</html>
```

#### **How the Code Works:**

1. **Vertex and Normal Data**: We define vertices with positions and normals in the `vertices` array. Each vertex consists of three values for position (x, y, z) and three values for the normal (nx, ny, nz).
2. **WebGL Setup**: We set up WebGL buffers for vertices and indices, compile shaders, and link them into a program.
3. **Rendering**: We use the `drawElements()` function to render the mesh based on the face indices (`indices` array).

#### **Conclusion**

In this tutorial, we've covered how to represent a mesh in WebGL using JavaScript, including:

* Defining vertices and normals in arrays.
* Calculating per-face and per-vertex normals for a triangle mesh.
* Rendering the mesh in WebGL with basic lighting using computed normals.
