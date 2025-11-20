# Step 4 Create an OpenGL window

## Add the following to main()

```cpp
int main()
{
    GLFWwindow *window;

    // GLFW init
    if (!glfwInit())
    {
        return -1;
    }

    window = glfwCreateWindow(640, 480, "Hello OpenGL1", NULL, NULL);
    glfwMakeContextCurrent(window);

    // GLAD init
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Couuldn't load opengl" << std::endl;
        glfwTerminate();
        return -1;
    }

    // set background colour
    glClearColor(0.25f, 0.5f, 0.75f, 1.0f);

    // rendering loop
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();

        glClear(GL_COLOR_BUFFER_BIT);

        glfwSwapBuffers(window);
    }

    glfwTerminate();

    return 0;
}
```

