# T2.3 Transform with GLM and connect to shader

## Github code

{% embed url="https://github.com/zhaoyoubing/proj02/releases/tag/tag_e_glm_rotation" %}

## GLM

GLM is the standard library for generating transformation matrix.

It is composed totally of header files so we only need to download it and copy it to the include directory.

Identity matrix

```cpp
glm::mat4 glm::mat4(1.0)
```

## GLM APIs for translation, scale and rotation

The common GLM APIs for translation, scale and rotation are

```cpp
glm::mat4 glm::translate(glm::vec3(tx, ty, tz));
glm::mat4 glm::rotate(glm::radians(angle_in_degree), glm::vec3(axis_x, axis_y, axis_z));
glm::mat4 glm::scale(glm::vec3(sx, sy, sz));
```

