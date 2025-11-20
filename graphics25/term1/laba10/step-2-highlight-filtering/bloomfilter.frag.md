# bloomfilter.frag

Create bloomfilter.frag under the **shaders** directory.&#x20;

### Input variables

We only need the texture coordinate and the texture itself.

```glsl
in vec2 texCoord;

// render-to-texture of the scene generated in the 1st pass
layout (binding=1) uniform sampler2D renderTex;
```

#### Scene rendering as a texture

From this shader, we start to use the 430 texture binding:

```glsl
// render-to-texture of the scene generated in the 1st pass
layout (binding=1) uniform sampler2D renderTex;
```

With this binding setting, you no longer need to set the texture unit with glUniform().

### Output variables

The only output variable we need is the ouput fragment colour.

```glsl
out vec4 colour_out;
```

## main()

### Read the pixel colour of the scene

First, read the pixel colour from the scene texture:

```glsl
vec4 colour = texture(renderTex, texCoord);
```

### Calculate the brightness

We are calculating the brightness of a pixel based on the following RGB coefficients:

```glsl
float brightness = dot(colour.rgb, vec3(0.2126, 0.7152, 0.0722));
```

### Filtering

In this fragment shader, we simply use a brightness threshold.

The following is an example using a thresholdof 0.9

```glsl
    // filter the bright part based on a threshold
    if(brightness > 0.9)
        colour_out = vec4(colour.rgb, 1.0);
    else
        colour_out = vec4(0.0, 0.0, 0.0, 1.0);
```

## Full Source Code

```glsl
// bloomfilter.frag
#version 430

in vec2 texCoord;

// render-to-texture of the scene generated in the 1st pass
layout (binding=1) uniform sampler2D renderTex;

out vec4 colour_out;

void main()
{
    vec4 colour = texture(renderTex, texCoord);

    // check whether fragment output is higher than threshold, if so output as brightness color
    float brightness = dot(colour.rgb, vec3(0.2126, 0.7152, 0.0722));
    
    // filter the bright part based on a threshold
    if(brightness > 0.9)
        colour_out = vec4(colour.rgb, 1.0);
    else
        colour_out = vec4(0.0, 0.0, 0.0, 1.0);
}
```
