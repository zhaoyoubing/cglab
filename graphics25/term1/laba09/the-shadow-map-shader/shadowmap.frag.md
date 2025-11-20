# shadowmap.frag

## Variables

### Inputs

add fragPosLightSpace

```glsl
in vec4 fragPosLightSpace;
```

### Uniforms

We need to add the depth shadowmap

```glsl
uniform sampler2D shadowMap;
```

## main()

at the end of main, replace the original colour\_out with the shadow\_calculation

```
    // calculate shadow using the light space position
    float shadow = ShadowCalculation(fragPosLightSpace);
    vec3 lighting = (ambient + (1.0 - shadow) * (diffuse + specular)) * colour;    
    
    colour_out = vec4(lighting, 1.0);
```

## ShadowCalculation()

Before main() define function ShadowCalculation(), which is the most important function in this lab.

The parameter of ShadowCalculation is the projected homogeneous position in the light space.

After the perspective division, the coordinates are in the Normalised Device Coordinate (NDC) space with a range of \[-1, 1] for all x, y and z.

With (x, y), we can query the depth value in the depth map.

By comparing z with the depth value, we can know if it is in the shadow : if z value is greater than the depth value in the lab, that fragment must be behind certain surfaces thus is in the shadow. &#x20;

```glsl
float ShadowCalculation(vec4 fragPosLightSpace)
{
    // perform perspective divide 
    // result: normalised device coordinates (NDC)
    // range [-1, 1] for x, y, z
    vec3 projCoords = fragPosLightSpace.xyz / fragPosLightSpace.w;
    
    // transform to [0,1] range
    projCoords = projCoords * 0.5 + 0.5;
    
    // get the depth value from the depth map 
    // corresponding to the closest visible part to the light source 
    float closeDepth = texture(shadowMap, projCoords.xy).r; 
    // get depth of current fragment from light's perspective
    float currentDepth = projCoords.z;
    
    // larger depth means far from the light source
    float shadow = currentDepth > closeDepth  ? 1.0 : 0.0;
    
    // You can also use bias
    // float bias = 0.005;
    //float shadow = currentDepth - bias > closestDepth  ? 1.0 : 0.0;
    // float bias = max(0.05 * (1.0 - dot(normal, lightDir)), 0.005);    
    
    return shadow;
}
```

## Full Source Code

```glsl
// shadowmap.frag
#version 410

in vec3 fragPos;
in vec3 normal;
in vec2 texCoord;
in vec4 fragPosLightSpace;

// uniform bool hasTexture;
uniform sampler2D textureMap;
uniform sampler2D shadowMap;

uniform vec3 lightPos;
uniform vec3 viewPos;

out vec4 colour_out;

float ShadowCalculation(vec4 fragPosLightSpace)
{
    // perform perspective divide 
    // result: normalised device coordinates (NDC)
    // range [-1, 1] for x, y, z
    vec3 projCoords = fragPosLightSpace.xyz / fragPosLightSpace.w;
    
    // transform to [0,1] range
    projCoords = projCoords * 0.5 + 0.5;
    
    // get the depth value from the depth map 
    // corresponding to the closest visible part to the light source 
    float closeDepth = texture(shadowMap, projCoords.xy).r; 
    // get depth of current fragment from light's perspective
    float currentDepth = projCoords.z;
    
    // larger depth means far from the light source
    float shadow = currentDepth > closeDepth  ? 1.0 : 0.0;
    
    // You can also use bias
    // float bias = 0.005;
    //float shadow = currentDepth - bias > closestDepth  ? 1.0 : 0.0;
    // float bias = max(0.05 * (1.0 - dot(normal, lightDir)), 0.005);    
    
    return shadow;
}

void main()
{   
    //vec3 colour = vec3(1.0, 0.0, 0.0);
    
    //if (hasTexture)
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
    
    // calculate shadow using the light space position
    float shadow = ShadowCalculation(fragPosLightSpace);
    vec3 lighting = ambient + (1.0 - shadow) * (diffuse + specular);    
    
    colour_out = vec4(lighting, 1.0);
}
```
