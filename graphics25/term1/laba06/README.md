# Lab A06 GGX Lighting with GenAI

GGX Specular (F0 = 0.04, roughness = 0.1)

<figure><img src="../../../.gitbook/assets/image (115).png" alt="" width="563"><figcaption></figcaption></figure>

Blinn-Phong (beta = 1000, light colour = vec3(1.0))

<figure><img src="../../../.gitbook/assets/image (117).png" alt="" width="563"><figcaption></figcaption></figure>

## The GGX Model

$$
f\left(\mathnormal{l}, \mathnormal{v} \right)=\frac{ D(\mathnormal{h}) F\left(\mathnormal{l}, \mathnormal{h}\right) G\left(\mathnormal{l}, \mathnormal{v}, \mathnormal{h}\right) }{4\left|\mathnormal{n} \cdot \mathnormal{l}\right|\left|\mathnormal{n} \cdot \mathnormal{v}\right|}
$$

$$
D(\mathnormal{h})= \frac{\alpha ^ 2}{\pi [ (n \cdot h)^2 (\alpha ^2 - 1) + 1] ^2}
$$

### Fresnel

$$
F(\mathnormal{l}, \mathnormal{h})=F_0+\left(1-F_0\right) *(1-(v \cdot h))^5 \quad
F_0=\frac{(n-1)^2}{(n+1)^2}
$$

### Smith One-sided Masking-Shadowing G1

$$
G_{1}(n \cdot v)= \frac{2}{1+\sqrt{1+\alpha^2 \tan ^2 \theta_v}} = \frac{2(n\cdot v)}{n \cdot v+\sqrt{\alpha^2+\left(1-\alpha^2\right)(n \cdot v)^2}}
$$

### Joint Masking-Shadowing G2

$$
G_{2}(n, l, v)=G_{1}(n \cdot l) G_{1}(n \cdot v)
$$

## Task 6.1 Generate the vertex shader and fragment shader&#x20;

Try to use GenAI tools to help generate the GGX fragment shader code.

## Task 6.2 Read the fragment shader functions

What kind of Normal Distribution Function and Smith masking-shadowing functions the source code is using ? They might be different from what is introduced the lecture.

Try to find correspondence between the variables in the code and in the GGX equation if your souce code is using functions similar to what has been introduced in the lecture.

### Fresnel function

### Normal distribution function (NDF)

### Smith Masking-Shadowing function G1 and G2

### Smith Geometry Function G1\*G2

### GGX function

## Task 6.3 Try to understand the backbone of the code conceptablly

## Task 6.4 Try to fit the code into your program and debug

Revise the Blinn-Phong fragment shader.&#x20;

Try to replace the specular component with GGX. Setting F0 and roughness.

Create new GGX shader variables in main.cpp and use the GGX shader to render your models.

Improve your understanding of GGX with code fitting and debugging.

## Task 6.5 Try to test different values of F0 and roughness

## Task 6.6 Try to compare GGX rendering with Blinn-Phong

