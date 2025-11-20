# LabA 03 Meshes and Scene Graphs

## Key points

Load mesh models

Draw mesh models

Organise models in a Scene Graph

## Source code

Startup project with Assimp (Vistudio 2022) with updated CMakeLists.txt and the model files

{% file src="../../../.gitbook/assets/Lab03_start_v_1_1_cmake_updated.zip" %}

## CMakeLists.txt Update

{% file src="../../../.gitbook/assets/CMakeLists (1).txt" %}

CMakeLists.txt has been updated to fix the copy dll bug.

Consistent relative path to shaders and models used by Visual Studio and VSCode are now supported.\
<mark style="color:red;">As this update is made after the completion of this lab guide, you need to manually update CMakeLists.txt and revise related code.</mark>

```cpp
// for both VSCode and Visual Studio
initShader( "shaders/colour.vert", "shaders/colour.frag");

//----------------------------------------------------
// Meshes
std::shared_ptr<Mesh> cube = std::make_shared<Mesh>();
cube->init("models/cube.obj", shader.program);


std::shared_ptr<Mesh> teapot = std::make_shared<Mesh>();
teapot->init("models/teapot.obj", shader.program);
```

## Downloads

cube.obj and teapot.obj, please put the models folder in your project folder

{% file src="../../../.gitbook/assets/models.zip" %}

<figure><img src="../../../.gitbook/assets/image (98).png" alt="" width="483"><figcaption></figcaption></figure>

## Tutorial

Scene graph

{% embed url="https://learnopengl.com/Guest-Articles/2021/Scene/Scene-Graph" %}

Loading model with Assimp (optional)

{% embed url="https://learnopengl.com/Model-Loading/Model" %}
