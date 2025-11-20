# bloomblend.frag

## Input variables

### The texture coordinate of the full-screen square

```glsl
in vec2 texCoord;
```

## The scene texture and the blurred highlight texture

```glsl
layout (binding=1) uniform sampler2D texrender;
layout (binding=2) uniform sampler2D texblur;
```

## Output variables

There is only one output variable: the fragment colour.

```glsl
out vec4 colour_out;
```

## main()

We are going to blend the the original rendering with the blurred highlights.

Accordingly, we read colour from two corresponding textures, one is the texture of the rendered scene, the other is the texture of the blurred highlights.

```glsl
    vec3 renderColour = texture(texrender, texCoord).rgb;      
    vec3 bloomColour = texture(texblur, texCoord).rgb;
```

#### Additive blending

A simple blending is using addition:

```glsl
    // additive blending
    vec3 blendColour = renderColour + bloomColour; 
```

#### High dyamic range (HDR) blending

But you may see the highlights are washed out because of the pixel values excceed 1.0f:

<figure><img src="../../../../.gitbook/assets/image (105).png" alt="" width="563"><figcaption></figcaption></figure>

We can use tone mapping and gamma correction to improve it, as described in https://learnopengl.com/Advanced-Lighting/HDR

```glsl
    const float gamma = 2.2;
    float exposure = 1.0;
    result = vec3(1.0) - exp(-result * exposure);
    // Gamma correction       
    result = pow(result, vec3(1.0 / gamma));
```

<figure><img src="../../../../.gitbook/assets/image (106).png" alt="" width="563"><figcaption></figcaption></figure>

## Full Source Code

```glsl
// bloomblend.frag
#version 430 core

in vec2 texCoord;

layout (binding=1) uniform sampler2D texrender;
layout (binding=2) uniform sampler2D texblur;

out vec4 colour_out;

void main()
{             
    vec3 renderColour = texture(texrender, texCoord).rgb;      
    vec3 bloomColour = texture(texblur, texCoord).rgb;
    
    // additive blending
    vec3 blendColour = renderColour + bloomColour; 
    
    vec3 result = blendColour;

    // =============================================
    // exposure tone mapping https://learnopengl.com/Advanced-Lighting/HDR
    /*
    const float gamma = 2.2;
    float exposure = 1.0;
    result = vec3(1.0) - exp(-result * exposure);
    // Gamma correction       
    result = pow(result, vec3(1.0 / gamma));
    */
    
    colour_out = vec4(result, 1.0);
} 
```

