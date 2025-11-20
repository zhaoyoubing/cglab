---
hidden: true
---

# WebGL

## Tasks

In this tutorial, we are going to cover

* Set up the WebGL context.
* Created shaders for rendering.
* Draw raw triangles, indexed triangles.
* Revised the shaders and WebGL code to draw indexed triangles with vertex colours.

## Step 1: Set up your HTML file and WebGL context

### Create an HTML file

Create an HTML file with a `<canvas>` element to render the WebGL content. We'll start by setting up the WebGL context.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WebGL Tutorial</title>
</head>
<body>
  <canvas id="webglCanvas" width="500" height="500"></canvas>
  <script>
  // TODO: put your WebGL code here
  </script>
</body>
</html>
```

### Set up the WebGL environment in JavaScript

In the \<script> \</script> section of LabA01\_triangle.html, start by initializing the WebGL context.

```javascript
const canvas = document.getElementById("webglCanvas");
const gl = canvas.getContext("webgl");

if (!gl) {
  alert("WebGL not supported");
}
```

## Create and Compile Shaders

### Creaet the Vertex and the Fragment Shader

Now we’ll create basic vertex and fragment shaders. The vertex shader will handle positioning, and the fragment shader will set the colour of the pixels.

```javascript
// Vertex shader source
const vertexShaderSource = `
  attribute vec4 a_position;
  void main(void) {
    gl_Position = a_position;
  }
`;

// Fragment shader source
const fragmentShaderSource = `
  precision mediump float;
  void main(void) {
    // set the R, G, B, A values of pixels
    // here we hard code the colour to RED
    gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
  }
`;
```

### Step 2 Compile shaders and link the program

Create functions to compile the shaders and link them into a shader program.

```javascript
function compileShader(gl, source, type) {
  const shader = gl.createShader(type);
  gl.shaderSource(shader, source);
  gl.compileShader(shader);
  if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
    console.error("Error compiling shader", gl.getShaderInfoLog(shader));
    gl.deleteShader(shader);
    return null;
  }
  return shader;
}

function initShaders(gl) {
  const vertexShader = compileShader(gl, vertexShaderSource, gl.VERTEX_SHADER);
  const fragmentShader = compileShader(gl, fragmentShaderSource, gl.FRAGMENT_SHADER);

  const program = gl.createProgram();
  gl.attachShader(program, vertexShader);
  gl.attachShader(program, fragmentShader);
  gl.linkProgram(program);

  if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
    console.error("Error linking program", gl.getProgramInfoLog(program));
    return null;
  }
  gl.useProgram(program);
  return program;
}

const shaderProgram = initShaders(gl);
```

## Step 3: Draw Raw or Indexed Triangles

### Draw raw triangles

#### Define triangle vertices

```javascript
const vertices = new Float32Array([
  0.0,  0.5,  // Top vertex
 -0.5, -0.5,  // Bottom-left vertex
  0.5, -0.5,  // Bottom-right vertex
]);

// we are drawing 2d triangles
const dim = 2;
```

#### Generate a vertex buffer.

```javascript
const vertexBuffer = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer);
gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW);

const positionLocation = gl.getAttribLocation(shaderProgram, "a_position");
gl.vertexAttribPointer(positionLocation, dim, gl.FLOAT, false, 0, 0);
gl.enableVertexAttribArray(positionLocation);

gl.clearColor(0.9, 0.9, 0.9, 1.0);
gl.clear(gl.COLOR_BUFFER_BIT);
gl.drawArrays(gl.TRIANGLES, 0, verticesSoup.length / dim);
```

### Draw multiple raw triangles

Now let’s define multiple triangles (a triangle soup) by adding more vertices. We’ll use the same process as before but with additional vertex data.

```javascript
const vertices = new Float32Array([
  // Triangle 1
  0.0,  0.5,
 -0.5, -0.5,
  0.5, -0.5,
  // Triangle 2
 -0.2,  0.7,
 -0.7,  0.0,
 -0.2, -0.7,
]);
```

## Draw Indexed Triangles

### Indexed triangles

```javascript
// shared vertices
const verticesIndexed = new Float32Array([
    0.0,  0.0,
    1.0,  0.0,
    1.0,  1.0,
    0.0,  1.0,
]);

// define the triangle by indices
const indices = new Uint16Array([
    0, 1, 2, 
    2, 3, 0
]);

```

For an indexed triangle, we define the vertices first and then refer to these vertices using indices.

```javascript
const vertexBufferIndexed = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, vertexBufferIndexed);
gl.bufferData(gl.ARRAY_BUFFER, verticesIndexed, gl.STATIC_DRAW);

// adding index buffer
const indexBuffer = gl.createBuffer();
gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, indexBuffer);
gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, indices, gl.STATIC_DRAW);

gl.clear(gl.COLOR_BUFFER_BIT);
gl.drawElements(gl.TRIANGLES, indices.length, gl.UNSIGNED_SHORT, 0);
```

## Drawing Triangles with Vertex Colours

### Adding Colour Attribute to Vertices

```javascript
// defining vertices
const vertices = new Float32Array([
    // (x, y)   (R, G, B)
    0.0,  0.0,  1.0, 0.0, 0.0,
    1.0,  0.0,  0.0, 1.0, 0.0,
    1.0,  1.0,  0.0, 0.0, 1.0,
    0.0,  1.0,  1.0, 1.0, 0.0
]);
```

### Setting Data Buffer Layout

Now each vertex has its own colour attribute. We use glVertexAttribPointer to tell the shader program this data layout.

For glVertexAttribPointer, its second parameter specifies the size of the attribute, its 5th parameter specifies the stride (number of floats for each vertex), and its last parameter specifies the pointer to the first element.

```javascript
void glVertexAttribPointer(index, size, type, normalized, stride, void * pointer);
```

For the (x, y) vertex position, we  need to inform the shader that it should find (x, y) coordinates every 5 float numbers.&#x20;

For the (r, g, b) vertex colour, we should inform the shader know that  the colour attribute start has a size of 3 and starts at the 3rd float number.

```javascript
// specify vertex position attribute in the vertex shader
const positionLocation = gl.getAttribLocation(shaderProgram, "a_position");
gl.vertexAttribPointer(positionLocation, 2, gl.FLOAT, false, 5 * Float32Array.BYTES_PER_ELEMENT, 0);
gl.enableVertexAttribArray(positionLocation);

// specify vertex colour attribute in the vertex shader
const colorAttribLocation = gl.getAttribLocation(shaderProgram, "aColor");
gl.vertexAttribPointer(colorAttribLocation, 3, gl.FLOAT, false, 5 * Float32Array.BYTES_PER_ELEMENT, 2 * Float32Array.BYTES_PER_ELEMENT);
gl.enableVertexAttribArray(colorAttribLocation);
```

### Drawing

The drawing JS code is the same as those for indexed triangles without colours.

```javascript
// clear background colour
gl.clearColor(0.9, 0.9, 0.9, 1.0);
gl.clear(gl.COLOR_BUFFER_BIT);

// indices.length is the 3 * triangle number
gl.drawElements(gl.TRIANGLES, indices.length, gl.UNSIGNED_SHORT, 0);
```

##

