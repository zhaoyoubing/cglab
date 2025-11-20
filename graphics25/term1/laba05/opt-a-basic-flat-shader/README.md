---
hidden: true
---

# \[opt] A Basic Flat Shader

For flat surface objects, such as the cube, sometimes we still like flat shading. In this section, we will introduce how to achieve flat shading via shader programming.

They are generally simpler than Phong and Blinn-Phong, the most critical different is to add a "flat" key word before the normal attributes so that GPUs will not use Phong shading normal interpolation.&#x20;
