# Vertex Shader

Changes in the vertex shader are straightforward by adding the texture coordinate input and ouput.

### Add the texture coordinate attribute input

```glsl
in layout(location=0) vec3 aPos;
in layout(location=1) vec3 aNormal;
// added for LabA07
in layout(location=2) vec2 aTexCoord;
```

### Adding the texure coordinate output

```glsl
out vec3 normal;
out vec3 fragPos;
// added for LabA07
out vec2 texCoord;
```

transfer texcoord from input to output in main()

<pre class="language-glsl"><code class="lang-glsl"><strong>void main()
</strong>{
    ...
    // added for LabA07
    texCoord = aTexCoord;
}
</code></pre>



## Full Source Code of the Vertex Shader

<pre class="language-glsl"><code class="lang-glsl">// texblinn.vert : Blinn-Phong shader with texture support
#version 410

in layout(location=0) vec3 aPos;
in layout(location=1) vec3 aNormal;
// added for LabA07
in layout(location=2) vec2 aTexCoord;

// M V P matrices
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

out vec3 normal;
out vec3 fragPos;
// added for LabA07
out vec2 texCoord;

<strong>void main()
</strong>{
    // homogeneous coordinate
    gl_Position = projection * view * model * vec4(aPos, 1.0); 
    
    fragPos = vec3(model * vec4(aPos, 1.0));

    // convert 4x4 modelling matrix to 3x3
    mat3 normalMatrix = mat3(model);
    
    // output normal to the fragment shader
    // !! only correct for rigid body transforms
    normal = normalMatrix * aNormal;

    // added for LabA07
    texCoord = aTexCoord;
}
</code></pre>
