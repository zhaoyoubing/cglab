# Step 10 Call triangle functions in main()

## Call triangle functions

Call initTriangle() after loading GLAD

Call drawTriangle in the event loop

```cpp
   // loading glad
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Couuldn't load opengl" << std::endl;
        glfwTerminate();
        return -1;
    }

    initTriangle();

    // setting the background colour, you can change the value
    glClearColor(0.25f, 0.5f, 0.75f, 1.0f);

    // setting the event loop
    while (!glfwWindowShouldClose(window))
    {
        glfwPollEvents();

        glClear(GL_COLOR_BUFFER_BIT);

        drawTriangle();

        glfwSwapBuffers(window);
    }

    glfwTerminate();
```

## Final result

Build and run the program, you will see a window similar to the following

<figure><img src="../../../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>



Try to play with the colour and triangle vertex coordinates.

Thinking about how to draw more than one triangle.
