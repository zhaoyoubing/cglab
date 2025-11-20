---
description: Drawing Triangles with Old OpenGL and GLUT (Immediate Mode)
---

# Early OpenGL

### Objective

This tutorial introduces rendering triangles using OpenGL with GLUT, employing the old immediate mode (`glBegin/glEnd`). We will start with a single triangle and extend it to multiple triangles.

### Prerequisites

* C++ development environment (e.g., Visual Studio, Code::Blocks, or GCC)
* OpenGL and GLUT installed

### &#x20;Early OpenGL

**Immediate mode** in OpenGL consists of `glBegin`/`glEnd` calls, with one or more `glVertex` calls (the minimum legal number depending on the _mode_ param of your `glBegin` call), and optionally other vertex attribute specification calls (`glColor`, `glTexCoord`, etc), between them.

* `glBegin` instructs the GL driver that you're starting to draw a point, line or polygon.
* `glTexCoord`, `glColor`, `glNormal`, etc just set a "current value" for the texture coordinate, colour, normal, etc.
* `glVertex` takes those "current values", together with the position information that you supply with the `glVertex` call, and transfers the lot to the driver.
* `glEnd` completes the point, line or polygon.

The major advantage to immediate mode is that you need to plan absolutely nothing up-front. You can just issue a `glBegin` call and start sending aribtrary geometry to your driver and hardware.

The major disadvantages to immediate mode include that it incurs a _lot_ of function call overhead into the GL driver, and that vertex data must _always_ be sent to the graphics hardware, even if that vertex data is unchanged from last time it was used.

***

### Step 1: Setting Up the Environment

1. Install GLUT if not already installed.
2. Create a new C++ project and include necessary headers:

```
#include <GL/glut.h>
#include <iostream>
```

3. Initialize GLUT and create a window:

```cpp
void init() {
    glClearColor(0.0f, 0.0f, 0.0f, 1.0f);
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glFlush();
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(800, 600);
    glutCreateWindow("OpenGL Triangle");
    init();
    glutDisplayFunc(display);
    glutMainLoop();
    return 0;
}
```

***

### Step 2: Drawing a Single Triangle

Modify the `display()` function to render a single triangle using `glBegin(GL_TRIANGLES)`:

```cpp
void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glLoadIdentity();
    
    glBegin(GL_TRIANGLES);
        glColor3f(1.0f, 0.0f, 0.0f);
        glVertex2f(0.0f, 0.5f);
        glColor3f(0.0f, 1.0f, 0.0f);
        glVertex2f(-0.5f, -0.5f);
        glColor3f(0.0f, 0.0f, 1.0f);
        glVertex2f(0.5f, -0.5f);
    glEnd();
    
    glFlush();
}
```

This will render a coloured triangle.

***

### Step 3: Drawing Multiple Triangles

Extend `display()` to draw multiple triangles:

```cpp
void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glLoadIdentity();

    glBegin(GL_TRIANGLES);
        glColor3f(1.0f, 0.0f, 0.0f);
        glVertex2f(-0.5f,  0.5f);
        glVertex2f( 0.0f, -0.5f);
        glVertex2f( 0.5f,  0.5f);
        
        glColor3f(0.0f, 1.0f, 0.0f);
        glVertex2f(-0.75f, -0.5f);
        glVertex2f(-0.25f, -0.5f);
        glVertex2f(-0.5f,   0.0f);
    glEnd();
    
    glFlush();
}
```

Now two triangles are drawn.

***

### Step 4: Drawing a Triangle Mesh with Indices

Although immediate mode does not support indexed rendering directly, we can conceptually structure our rendering using indices in an array and manually reference them:

```cpp
void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glLoadIdentity();

    GLfloat vertices[][2] = {
        {-0.5f, -0.5f}, {0.5f, -0.5f}, {0.5f, 0.5f}, {-0.5f, 0.5f}
    };

    GLint indices[][3] = {
        {0, 1, 2},
        {2, 3, 0}
    };

    glBegin(GL_TRIANGLES);
    for (int i = 0; i < 2; i++) {
        glColor3f((i + 1) % 2, i % 2, 1.0f);
        for (int j = 0; j < 3; j++) {
            glVertex2fv(vertices[indices[i][j]]);
        }
    }
    glEnd();

    glFlush();
}
```

This example simulates indexed rendering by manually referencing vertex data, drawing two triangles to form a quad.

***

### Conclusion

This tutorial demonstrated how to render triangles using OpenGL’s immediate mode with GLUT. Future steps include transitioning to modern OpenGL (VBOs and shaders).
