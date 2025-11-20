# Revise Mesh::draw()

Now we need to set the texture map uniform in draw() and activate the texture map.

```cpp
    // added in LabA07
    // =====================================================
    GLint textureLoc = glGetUniformLocation(shaderId, "textureMap");
    glUniform1i(textureLoc, 0); 
    // =====================================================
```

## Full Source Code of Mesh::draw()

```cpp
void Mesh::draw(glm::mat4 matModel, glm::mat4 matView, glm::mat4 matProj)
{
    // 1. Bind the correct shader program
    glUseProgram(shaderId);

    // 2. Set the appropriate uniforms for each shader
    // set the modelling transform  
    GLuint model_loc = glGetUniformLocation(shaderId, "model" );
    glUniformMatrix4fv(model_loc, 1, GL_FALSE, &matModel[0][0]);

    // set view matrix
    GLuint view_loc = glGetUniformLocation(shaderId, "view" );
    glUniformMatrix4fv(view_loc, 1, GL_FALSE, &matView[0][0]);

    // set projection transforms
    glm::mat4 mat_projection = matProj;
    GLuint projection_loc = glGetUniformLocation( shaderId, "projection" );
    glUniformMatrix4fv(projection_loc, 1, GL_FALSE, &mat_projection[0][0]);

    // added in LabA07
    // =====================================================
    GLint textureLoc = glGetUniformLocation(shaderId, "textureMap");
    glUniform1i(textureLoc, 0); 
    // =====================================================


    // 3. Bind the corresponding model's VAO
    glBindVertexArray(buffers[0]);

    // 4. Draw the model
    glDrawElements(GL_TRIANGLES, indices.size(), GL_UNSIGNED_INT, 0);

    // 5. Unset vertex buffer
    glBindVertexArray(0);
}
```
