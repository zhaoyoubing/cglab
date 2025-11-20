# Revise Mesh::draw() to use normal map

### Set the normalMap uniform and bind normal map after texture map in Mesh::draw()

```cpp
void Mesh::draw(glm::mat4 matModel, glm::mat4 matView, glm::mat4 matProj)
{
    // 1. Bind the correct shader program
    glUseProgram(shaderId);
    ...
    
    GLint textureLoc = glGetUniformLocation(shaderId, "textureMap");
    glUniform1i(textureLoc, 0); 

    if (! textures.empty()) {
        glActiveTexture(GL_TEXTURE0);
        glBindTexture(GL_TEXTURE_2D, textures[0].id);
    }

    // for LabA08 Normal Map
    GLint normalmapLoc = glGetUniformLocation(shaderId, "normalMap");
    glUniform1i(normalmapLoc, 1); 

    if (! normals.empty()) {
        // Texture mapping, we only deal with one texture unit    
        glActiveTexture(GL_TEXTURE1);
        glBindTexture(GL_TEXTURE_2D, normals[0].id);
    }
    
    ...
}
```
