# T2.1 Draw two triangles using vertex indices

## Github code

{% embed url="https://github.com/zhaoyoubing/proj02/releases/tag/tag_a_two_triangle" %}

Now we are drawing two triangles.&#x20;

<figure><img src="../../../../.gitbook/assets/a53c489c-9df7-4785-afff-19ecb9b7822b.png" alt=""><figcaption></figcaption></figure>

One way is to use the method in Lab 01 to draw two triangles. However, it is not very efficient as you have to duplicate vertices.

```cpp
    GLfloat verts[] = {
        -1.0f, 1.0f,  // v0
        -1.0f, -1.0f, // v1
        1.0f, -1.0f,  // v2

        1.0f, -1.0f,  // v2
        1.0f, 1.0f,   // v3
        -1.0f, 1.0f,  // v0
    };
```

This week we are going to learn use vertex with indices to draw multiple triangles.
