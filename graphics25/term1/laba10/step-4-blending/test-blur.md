# Test blur()



in bloomblend.frag, if we set the output to be the blur texture, then we can see the result of blurring.

```glsl
    // set result = bloomColour to test blurring
    vec3 result = bloomColour;
    // vec3 result = blendColour;
```

### Blur on a single direction

#### Blur on the X direction (50 passes):

<figure><img src="../../../../.gitbook/assets/image (2).png" alt="" width="563"><figcaption></figcaption></figure>

#### Blur on the Y direction (50 passes):

<figure><img src="../../../../.gitbook/assets/image (3).png" alt="" width="563"><figcaption></figcaption></figure>



### Number of iterations

#### 10 passes

<figure><img src="../../../../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 50 passes

<figure><img src="../../../../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 500 passes

<figure><img src="../../../../.gitbook/assets/image (4).png" alt="" width="563"><figcaption></figcaption></figure>

