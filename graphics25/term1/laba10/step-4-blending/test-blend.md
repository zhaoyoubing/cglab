# Test blend()

If blurring is OK we are just one step from blending.

### Now in bloomblend.frag, let's move back to use the blend colour.

```glsl
    // set "result = bloomColour" to test blurring
    // vec3 result = bloomColour;
    vec3 result = blendColour;
```

## Result

### Simple Additive

If you use simple additive blending, you are going to see something similar to the following&#x20;

### 10 blur passes:

<figure><img src="../../../../.gitbook/assets/image (111).png" alt="" width="563"><figcaption></figcaption></figure>

#### 50 blur passes

<figure><img src="../../../../.gitbook/assets/image (114).png" alt="" width="563"><figcaption></figcaption></figure>

### High Dynamic Range

If you use High Dynamic Range, you will see something similar to the following

#### &#x20;10 blur passes:

<figure><img src="../../../../.gitbook/assets/image (112).png" alt="" width="563"><figcaption></figcaption></figure>

50 blur passes

<figure><img src="../../../../.gitbook/assets/image (113).png" alt="" width="563"><figcaption></figcaption></figure>
