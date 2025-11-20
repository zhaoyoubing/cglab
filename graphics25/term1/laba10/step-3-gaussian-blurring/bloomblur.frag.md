# bloomblur.frag

## Input Variables

Texture coord and the framebuffer texture for blurring

```glsl
in vec2 texCoord;
layout (binding=2)  uniform sampler2D texblur;
```

### Weight for one dimensional Gaussian blurring

weight\[0] is the weight for the current pixel, weight\[i] for its left i and right i pixel respectively.

```glsl
uniform float weight[5] = float[] (0.227027, 0.1945946, 0.1216216, 0.054054, 0.016216);
```

### pass

We are going to iteratively apply the horizaontal and vertical blurring, so we need a pass number.

If the pass is odd, we think it is a horizontal blurring, otherwise a vertcal.

```glsl
uniform int pass;
```

## Output variables

We only need the output fragment colour.

```glsl
out vec4 colour_out;
```

## main()

### Get the pixel colour from previous iteration (filtering or blurring)

```glsl
// current fragment's contribution        
vec3 result = texture(texblur, texCoord).rgb * weight[0]; 
```

### Get the size of a single texel

```glsl
vec2 tex_offset = 1.0 / textureSize(texblur, 0); // gets size of single texel
```

### Horizontal blurring

If the **pass** number is odd, we are going to perform a horizontal blurring by weighted averaging.

```glsl
    if (pass % 2== 0)
    {
        // horizontal on the x direction
        for(int i = 1; i < 5; ++i)
        {
            result += texture(texblur, texCoord + vec2(tex_offset.x * i, 0.0)).rgb * weight[i];
            result += texture(texblur, texCoord - vec2(tex_offset.x * i, 0.0)).rgb * weight[i];
        }
    }
```

### Vertical blurring

If the **pass** number is even, we are going to perform a horizontal blurring by weighted averaging.

```
    if (pass % 2 == 0)
    {
        ...
    }
    else
    {
        // vertical on the y direction  
        for(int i = 1; i < 5; ++i)
        {
            result += texture(texblur, texCoord + vec2(0.0, tex_offset.y * i)).rgb * weight[i];
            result += texture(texblur, texCoord - vec2(0.0, tex_offset.y * i)).rgb * weight[i];
        }
    }
```

### Finally return the fragment colour

<pre class="language-glsl"><code class="lang-glsl"><strong>main()
</strong><strong>{
</strong><strong>    ...
</strong><strong>    colour_out = vec4(result, 1.0);
</strong><strong>}
</strong></code></pre>

## Full Source Code

```glsl
// bloomblur.frag
#version 430
  
in vec2 texCoord;
layout (binding=2)  uniform sampler2D texblur;
uniform float weight[5] = float[] (0.227027, 0.1945946, 0.1216216, 0.054054, 0.016216);

uniform int pass;

out vec4 colour_out;

void main()
{   
    // current fragment's contribution 
    vec3 result = texture(texblur, texCoord).rgb * weight[0];  
    // gets size of single texel 
    vec2 tex_offset = 1.0 / textureSize(texblur, 0);     

    if ( pass % 2 == 0)
    {
        // horizontal on the x direction
        for(int i = 1; i < 5; ++i)
        {
            result += texture(texblur, texCoord + vec2(tex_offset.x * i, 0.0)).rgb * weight[i];
            result += texture(texblur, texCoord - vec2(tex_offset.x * i, 0.0)).rgb * weight[i];
        }
    }
    else
    {
        // vertical on the x direction  
        for(int i = 1; i < 5; ++i)
        {
            result += texture(texblur, texCoord + vec2(0.0, tex_offset.y * i)).rgb * weight[i];
            result += texture(texblur, texCoord - vec2(0.0, tex_offset.y * i)).rgb * weight[i];
        }
    }

    colour_out = vec4(result, 1.0);
}
```
