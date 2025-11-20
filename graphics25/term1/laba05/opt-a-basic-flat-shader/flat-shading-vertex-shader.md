# Flat Shading Vertex Shader

## Create the flat shading vertex shader

Create **flat.vert** in folder **shaders**.

## Input Vertex attributes

Similar to Blinn-Phong and Phong, we are using vertex positions and vertex normals.

```glsl
#version 410

in layout(location=0) vec3 pos;
in layout(location=1) vec3 aNormal;
```

## Uniforms

Our uniforms are still three martrices of modelling, viewing and projection.

```glsl
// Model View Projection
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;
```

## Output variables to the fragment shader

The output variables are still the fragment position and the normal vector in the world space.

The most import difference is that there is a "flat" keyword before normal. With the "flat" keyword, the normals of the fragments will not be based on Phong shading interpolation.

```glsl
// !! don't forget the "flat" modifier
out flat vec3 normal;
out vec3 fragPos;
```

## main()

The main() function is very similar to that of Blinn-Phong and Phong. The fragment position and the normal vector in the world space are calculated.&#x20;

```glsl
void main()
{
    // homogeneous coordinate
    gl_Position = projection * view * model * vec4(pos, 1.0); 
    
    fragPos = vec3(model * vec4(pos, 1.0));
    
    // convert 4x4 modelling matrix to 3x3
    mat3 normalMatrix = mat3(model);

    // output normal to the fragment shader
    // !! only correct for rigid body transforms
    normal = normalMatrix * aNormal;
}
```

## Full Source Code

```glsl
// flat.vert
#version 410

in layout(location=0) vec3 pos;
in layout(location=1) vec3 aNormal;

// Model View Projection
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// !! don't forget the "flat" modifier
out flat vec3 normal;
out vec3 fragPos;

void main()
{
    // homogeneous coordinate
    gl_Position = projection * view * model * vec4(pos, 1.0); 
    
    fragPos = vec3(model * vec4(pos, 1.0));
    
    // convert 4x4 modelling matrix to 3x3
    mat3 normalMatrix = mat3(model);

    // output normal to the fragment shader
    // !! only correct for rigid body transforms
    normal = normalMatrix * aNormal;
}
```
