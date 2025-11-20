# Flat Shading Fragment Shader

## Create the flat shading fragment shader

Create **flat.frag** in folder **shaders**.

## The difference

The fragment shader of flat shading is simpler. As there is no specular reflections, it does not need the eye position.

## Input variables

The output variables of the vertex shader are the input variables of the fragment shader. Accordingly, we have the input variables of the flat normal and the fragment position.

```glsl
#version 410

// !! don't forget the "flat" modifier
in flat vec3 normal;
in vec3 fragPos;
```

## Uniforms

Because the eye position is not needed, there is only one uniform: position of the light source

```glsl
uniform vec3 lightPos;

// not needed for flat shading
//uniform vec3 viewPos;
```

## Output variables

The only output of the fragment shader is the fragment colour.

```glsl
out vec4 colour_out;
```

## The main() function



