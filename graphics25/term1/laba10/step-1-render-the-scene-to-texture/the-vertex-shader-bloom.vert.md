# The Vertex Shader bloom.vert

The vertex shader is shared by all fragment shaders.

I copied texblinn.vert to bloom.vert under the **shaders** directory.&#x20;

The only change I made is to change the version from 410 to 430 to facilitate texture uniforms. However, if you are using MacOS, you should stick to 410.

```glsl
// bloom.vert
#version 430

in layout(location=0) vec3 aPos;
in layout(location=1) vec3 aNormal;
in layout(location=2) vec2 aTexCoord;

// M V P matrices
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

out vec3 fragPos;
out vec3 normal;
out vec2 texCoord;

void main()
{
    // homogeneous coordinate
    gl_Position = projection * view * model * vec4(aPos, 1.0); 
    
    fragPos = vec3(model * vec4(aPos, 1.0));

    // convert 4x4 modelling matrix to 3x3
    mat3 normalMatrix = mat3(model);
    
    // output normal to the fragment shader
    // !! only correct for rigid body transforms
    normal = normalMatrix * aNormal;

    texCoord = aTexCoord;
}
```
