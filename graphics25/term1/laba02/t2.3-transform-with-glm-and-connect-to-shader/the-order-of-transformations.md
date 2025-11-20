# The order of transformations

Different orders of transformations, espeically translation and rotation, have different results



## M = RTS

scale(0.5), then translate(0.3, 0.2, 0.0), then rotate 60 degrees about  Z Axis

```cpp
    glm::mat4 mat_scale = glm::scale(glm::vec3(0.5f, 0.5f, 0.5f));
    glm::mat4 mat_trans = glm::translate(glm::vec3(0.3f, 0.2f, 0.0f));
    glm::mat4 mat_rot = glm::rotate(glm::radians(60.0f), glm::vec3(0.0f, 0.0f, 1.0f));

    glm::mat4 mat_modelview = mat_rot * mat_trans * mat_scale;
```

<figure><img src="../../../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

## M = TRS

cale(0.5), then rotate 60 degrees about  Z Axis, then translate(0.3, 0.2, 0.0),&#x20;

```cpp
    // the order matters
    glm::mat4 mat_modelview = mat_trans * mat_rot * mat_scale;
```

<figure><img src="../../../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>
