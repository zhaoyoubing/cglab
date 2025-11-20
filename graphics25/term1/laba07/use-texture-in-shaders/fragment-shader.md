# Fragment Shader

## Fragment Shader Variables

### Fragment Shader Input

```glsl
in vec3 fragPos;
in vec3 normal;
// added for LabA07
in vec2 texCoord;
```

### Fragment Shader Uniforms

We are adding the texture map as a uniform.

Uniforms of sampler types are used in GLSL to represent a texture of a particular kind. Therefore, sampler types represent textures.

This texture uniform corresponds to the texture unit used in glActiveTexture(GL\_TEXTURE#).

For example, 0 is GL\_TEXTURE0, 1 is GL\_TEXTURE1.

```glsl
uniform vec3 lightPos;
uniform vec3 viewPos;

// LabA07 texture map unit id
uniform sampler2D textureMap;
```

Note: From OpenGL 4.3, we can use the following to directly refer to a texture unit without setting it with glUniform()

```glsl
layout(binding = 0) uniform sampler2D textureMap;
```

### Fragment shader ouput unchanged

```glsl
out vec4 colour_out;
```

## Fragment Shader main()

The only change in main is to read colour from the texture map.

```glsl
void main()
{
    // changed for LabA07
    vec3 colour = texture(textureMap, texCoord).rgb;
    
    ...
}
```

## Full Source Code fo the Fragment Shader

```glsl
// texblinn.frag : Blinn-Phong shader with texture support
#version 410

// for lighting
in vec3 fragPos;
in vec3 normal;
// added for LabA07
in vec2 texCoord;

uniform vec3 lightPos;
uniform vec3 viewPos;

// LabA07 diffuse texture map only
uniform sampler2D textureMap;

out vec4 colour_out;

void main()
{
    // changed for LabA07
    vec3 colour = texture(textureMap, texCoord).rgb;

    // 1. ambient
    vec3 ambient = 0.05 * colour;

    // 2. diffuse
    vec3 lightDir = normalize(lightPos - fragPos);
    vec3 norm = normalize(normal);
    float diff = max(dot(lightDir, norm), 0.0);
    vec3 diffuse = diff * colour;
    
    // 3. specular
    vec3 viewDir = normalize(viewPos - fragPos);
    vec3 halfwayDir = normalize(lightDir + viewDir);
    
    // the shininess coefficient beta = 32.0 
    float spec = pow(max(dot(norm, halfwayDir), 0.0), 32.0);

    // assuming a light source with a bright white colour
    vec3 specular = vec3(0.3) * spec;

    // The final output fragment colour 
    // combination of ambient, diffuse and specular
    colour_out = vec4(ambient + diffuse + specular, 1.0);
}
```
