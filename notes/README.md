---
hidden: true
---

# Notes

## Lighting

{% embed url="https://en.wikipedia.org/wiki/Oren%E2%80%93Nayar_reflectance_model" %}

{% embed url="https://www.modelical.com/en/gdocs/materials-pbr-materials-inputs-material-editor-within-ue/" %}

{% embed url="https://www.youtube.com/watch?v=XK_p2MxGBQs&t=483s" %}



[https://people.computing.clemson.edu/\~ekp/courses/dpa8090/](https://people.computing.clemson.edu/~ekp/courses/dpa8090/)

{% embed url="https://www.youtube.com/watch?v=wbBtAFpOxg8&t=1772s" %}

[https://www.adobe.com/learn/substance-3d-designer/web/the-pbr-guide-part-1?learnIn=1](https://www.adobe.com/learn/substance-3d-designer/web/the-pbr-guide-part-1?learnIn=1)



cmake\_minimum\_required(VERSION 3.14) project(MyApp)

{% embed url="https://ycpcs.github.io/cs370-fall2025/labs/index.html" %}

##

## Set up your executable

add\_executable(MyApp src/main.cpp)

## Add glad

add\_library(glad STATIC external/glad/glad.c) target\_include\_directories(glad PUBLIC external/glad/include)

## Link glad to your app

target\_link\_libraries(MyApp PRIVATE glad)

#### WebGL startup example

{% embed url="https://web.cs.upb.de/cgvb/WebGLEditor/" %}

<figure><img src="../.gitbook/assets/3eb2e241-4043-49fa-8a2c-9f57af8e5b9d.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/8f64791e-eebd-46af-921f-3550484f04f8.png" alt=""><figcaption></figcaption></figure>

