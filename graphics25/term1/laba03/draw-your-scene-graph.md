---
hidden: true
---

# Draw your scene graph

Multiple models (`model1`, `model2`), each with its own VAO

* **Bind the correct shader program** before drawing each model.
* **Bind the corresponding model's VAO/VBO/etc.**
* **Set the appropriate uniforms** for each shader (like transformation matrices, lighting, etc.)
* **Draw the model**.



```cpp
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

    // ========== Draw Model 1 with Shader 1 ==========
    glUseProgram(shaderProgram1);

    // Set uniforms (e.g., transformation matrices)
    glm::mat4 modelMatrix1 = ...;
    glm::mat4 viewMatrix = ...;
    glm::mat4 projectionMatrix = ...;

    glUniformMatrix4fv(glGetUniformLocation(shaderProgram1, "model"), 1, GL_FALSE, glm::value_ptr(modelMatrix1));
    glUniformMatrix4fv(glGetUniformLocation(shaderProgram1, "view"), 1, GL_FALSE, glm::value_ptr(viewMatrix));
    glUniformMatrix4fv(glGetUniformLocation(shaderProgram1, "projection"), 1, GL_FALSE, glm::value_ptr(projectionMatrix));

    // Bind VAO and draw
    glBindVertexArray(model1VAO);
    glDrawElements(GL_TRIANGLES, model1IndexCount, GL_UNSIGNED_INT, 0);

    // ========== Draw Model 2 with Shader 2 ==========
    glUseProgram(shaderProgram2);

    glm::mat4 modelMatrix2 = ...;

    glUniformMatrix4fv(glGetUniformLocation(shaderProgram2, "model"), 1, GL_FALSE, glm::value_ptr(modelMatrix2));
    glUniformMatrix4fv(glGetUniformLocation(shaderProgram2, "view"), 1, GL_FALSE, glm::value_ptr(viewMatrix));
    glUniformMatrix4fv(glGetUniformLocation(shaderProgram2, "projection"), 1, GL_FALSE, glm::value_ptr(projectionMatrix));

    glBindVertexArray(model2VAO);
    glDrawElements(GL_TRIANGLES, model2IndexCount, GL_UNSIGNED_INT, 0);

    // etc...

    glfwSwapBuffers(window);
    glfwPollEvents();
```
