# The Depth Shader

Now we are writing the depth shader to calcualte the depth from the light source.

Create simpledepth.vert and simpledepth.frag under the **shaders** directory.

## The Vertex Shader

We only need M V P marices to calculate gl\_Position in the vertex shader.

Remember, the view and projection matrices are from the light source instead of the eye.

```glsl
// simpledepth.vert
#version 410
layout (location = 0) in vec3 aPos;

uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
}  
```

## The Fragment Shader

There is nothing actually needed in the fragment shader as we are not going to draw any colour pixels.

However, to help us debug we can render the colour as the depth. Darker colours correspond to smaller value in Z and closer to the light source.

```glsl
// simpledepth.frag
#version 410

out vec4 fragColour;

void main()
{   
    // code to show the depth buffer, can be commented
    float d = gl_FragCoord.z;
    fragColour = vec4(d, d, d, 1.0);
} 
```
