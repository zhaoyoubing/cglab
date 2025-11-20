# 5.2 Blinn-Phong Fragment Shader

Now we are going to implement the Blinn-Phong Fragment Shader

## Create blinn.frag

Create blinn.frag in folder **shaders**.

## Declare Input variables from the vertex shader

First we are going to delare the variables from the vertex shader. They are the fragment position in the world space (fragPos) and the normal vector in the world space (normal).&#x20;

```glsl
#version 410

// for lighting
in vec3 fragPos;
in vec3 normal;
```

## Declare Uniforms

The uniforms required for light computing are the light source position (lightPos) and the camera position (viewPos).

```glsl
uniform vec3 lightPos;
uniform vec3 viewPos;
```

## Declare output variables

The only ouput is the colour of the fragment (pixel)

```glsl
out vec4 colour_out;
```

## The main() function

Note: In this lab we are using hardcoded surface colour, light source colour, ambient, diffuse and specular coefficients.&#x20;

This is not a good practice and should be replaced by material parameters and textures in the future.

### Set the surface colour

Here we are hardcoding the diffuse surface colour. In the future, it is going to be replace by the colour from the texture.

```glsl
void main()
{
    // manually set R G B of the light colour, here it is RED
    vec3 colour = vec3(1.0, 0.0, 0.0);
    
    ...
}
```

### The ambient component

here, the ambient component is simply based on the surface colour.

```glsl
void main()
{
    // manually set R G B of the light source colour, here is RED
    vec3 colour = vec3(1.0, 0.0, 0.0);

    // 1. ambient
    vec3 ambient = 0.05 * colour;
    
    ...
}
```

### The diffuse component

The diffuse component is calculated based on the cosine of the angle between the light direction and the surface normal. It is independant from the view direction.

```glsl
void main()
{
    ...
    // 2. diffuse
    vec3 lightDir = normalize(lightPos - fragPos);
    vec3 norm = normalize(normal);
    float diff = max(dot(lightDir, norm), 0.0);
    vec3 diffuse = diff * colour;
    ...
}
```

### The specular component

In Blinn-Phong model, we first calculate the half vector, then use the cosine of the angle between the surface normal and the half vector to compute the specular light reflected.

Here we are using a shininess coefficient $$\beta$$ of 32.0 for&#x20;

```glsl
void main() 
{
...
    // 3. specular
    vec3 viewDir = normalize(viewPos - fragPos);
    vec3 reflectDir = reflect(-lightDir, normal);
    vec3 halfwayDir = normalize(lightDir + viewDir);
    
    // the shininess coefficient beta = 32.0 
    float spec = pow(max(dot(norm, halfwayDir), 0.0), 32.0);
...
}
```

We  assume the light colour of the light source is white, but you can definitely try other colours

```glsl
void main() 
{
...
    // 3. specular
    vec3 viewDir = normalize(viewPos - fragPos);
    vec3 halfwayDir = normalize(lightDir + viewDir);
    
    // the shininess coefficient beta = 32.0 
    float spec = pow(max(dot(norm, halfwayDir), 0.0), 32.0);
    
    // assuming a light source with a bright white colour
    vec3 specular = vec3(0.3) * spec;
...
}
```

## The output colour

The output fragment colour is just the combination of the ambient, diffuse and specular components.

```glsl
void main() 
{
    ...
    
    // The final output fragment colour 
    // combination of ambient, diffuse and specular
    colour_out = vec4(ambient + diffuse + specular, 1.0);
}
```

## Result

We need to set the vertex shader and fragment file names in initShader() in main() to **blinn.vert** and **blinn.frag**.

```cpp
// in main()
initShader( "shaders/blinn.vert", "shaders/blinn.frag");
```

With the following light source position of glm::vec3(5.0f, 5.0f, 10.0f) and  eye position of  glm::vec3(0.0f, 0.0f, 5.0f) in main.cpp, you will see something similar to the following:

<figure><img src="../../../../.gitbook/assets/image (16).png" alt="" width="483"><figcaption></figcaption></figure>

## Full Source Code

```glsl
// blinn.frag
#version 410

// for lighting
in vec3 fragPos;
in vec3 normal;

uniform vec3 lightPos;
uniform vec3 viewPos;

out vec4 colour_out;

void main()
{
    // manually set R G B of the surface colour, here is RED
    vec3 colour = vec3(1.0, 0.0, 0.0);

    // 1. ambient
    vec3 ambient = 0.05 * colour;

    // 2. diffuse
    vec3 lightDir = normalize(lightPos - fragPos);
    vec3 norm = normalize(normal);
    float diff = max(dot(lightDir, normal), 0.0);
    vec3 diffuse = diff * color;
    
    // 3. specular
    vec3 viewDir = normalize(viewPos - fragPos);
    vec3 reflectDir = reflect(-lightDir, normal);
    vec3 halfwayDir = normalize(lightDir + viewDir);
    
    // the shininess coefficient beta = 32.0 
    float spec = pow(max(dot(normal, halfwayDir), 0.0), 32.0);

    // assuming a light source with a bright white colour
    vec3 specular = vec3(0.3) * spec;

    // The final output fragment colour 
    // combination of ambient, diffuse and specular
    colour_out = vec4(ambient + diffuse + specular, 1.0);
}
```
