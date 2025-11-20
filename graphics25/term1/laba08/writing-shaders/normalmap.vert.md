# normalmap.vert

## Variables and Uniforms

### Input variables and Uniforms

#### add tangent space in variables

```glsl
// added for LabA08 Normal Map
in layout(location=3) vec3 aTangent;
in layout(location=4) vec3 aBitangent;
```

#### Uniforms

Move lightpos and viewpos from the fragment shader to the vertex shader as we are going to calculate lighting in the tangent space

```glsl
uniform vec3 lightPos;
uniform vec3 viewPos;
```

### Outpu variables

#### add tangent space out variables

```glsl
out vec3 tangentLightPos;
out vec3 tangentViewPos;
out vec3 tangentFragPos;
```

#### comment out normal

```glsl
//out vec3 normal;
```

## in main()

### Comment out the normal transform&#x20;

as we are going to read the normal from the normal map in the fragment shader.

```glsl
    // commented out for LabA08 
    // mat3 normalMatrix = mat3(model);
    // normal = normalMatrix * aNormal;
```

### Generate the inverse (transpose) of the tangent space matrix TBN

```glsl
    vec3 T = normalize(vec3(model * vec4(aTangent,   0.0)));
    vec3 N = normalize(vec3(model * vec4(aNormal,    0.0)));
    // B can also be calculated by : vec3 B = normalize(cross(N, T));
    vec3 B = normalize(vec3(model * vec4(aBitangent, 0.0)));
    
    mat3 invTBN = transpose(mat3(T, B, N));
```

### Transform lightPos, viewPos and fragPos to the tangent space

```glsl
    tangentLightPos = invTBN * lightPos;
    tangentViewPos  = invTBN * viewPos;
    tangentFragPos  = invTBN * vec3(model * vec4(aPos, 1.0));
```

## Full Source Code

```glsl
// LabA08 Normal Map
// normalmap.vert
#version 410

in layout(location=0) vec3 aPos;
in layout(location=1) vec3 aNormal;
in layout(location=2) vec2 aTexCoord;

// added for LabA08 Normal Map
in layout(location=3) vec3 aTangent;
// here we directly use bitangent from ASSIMP
// it can also be calculted by cross product
in layout(location=4) vec3 aBitangent;

// M V P matrices
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// moved from the fragment shader for LabA08 Normal Map
uniform vec3 lightPos;
uniform vec3 viewPos;

// commented out for LabA08 Normal Map
//out vec3 normal;
out vec3 fragPos;
out vec2 texCoord;

// added for LabA08 Normal Map
out vec3 tangentLightPos;
out vec3 tangentViewPos;
out vec3 tangentFragPos;

void main()
{
    // homogeneous coordinate
    gl_Position = projection * view * model * vec4(aPos, 1.0); 
    
    fragPos = vec3(model * vec4(aPos, 1.0));

    texCoord = aTexCoord;

    vec3 T = normalize(vec3(model * vec4(aTangent,   0.0)));
    vec3 N = normalize(vec3(model * vec4(aNormal,    0.0)));
    // B can also be calculated by : vec3 B = normalize(cross(N, T));
    vec3 B = normalize(vec3(model * vec4(aBitangent, 0.0)));
    
    mat3 invTBN = transpose(mat3(T, B, N));

    tangentLightPos = invTBN * lightPos;
    tangentViewPos  = invTBN * viewPos;
    tangentFragPos  = invTBN * vec3(model * vec4(aPos, 1.0));
}
```
