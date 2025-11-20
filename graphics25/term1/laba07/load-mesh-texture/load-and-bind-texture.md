# Load and Bind Texture

## Read Texture from an Image

### Copy Mesh::loadMaterialTextures to your Mesh.cpp

```cpp
// added in LabA07
// =====================================================
std::vector<Texture> Mesh::loadMaterialTextures(aiMaterial *mat, aiTextureType type, std::string typeName, std::string dir)
{
    std::vector<Texture> textures;

    // actually, we only support one texture
    int nTex = mat->GetTextureCount(type);
    for(unsigned int i = 0; i < nTex ; i++)
    {
        aiString str;
        mat->GetTexture(type, i, &str);

        Texture texture;
        texture.id = loadTextureAndBind(str.C_Str(), dir);
        texture.type = typeName;
        if (texture.id > 0)
            textures.push_back(texture);
    }
    return textures;
}  
```

### Read Texture Image

### Create method Mesh::loadTextureAndBind()

```cpp
unsigned int Mesh::loadTextureAndBind(const char* path, const std::string& directory)
```

### read texture image data with stbi\_load()

```cpp
unsigned int Mesh::loadTextureAndBind(const char* path, const std::string& directory)
{
    int width, height, nrComponents;
    unsigned char* data = stbi_load(filename.c_str(), &width, &height, &nrComponents, 0);
    if (! data) {
        std::cout << "Texture failed to load at path: " << path << std::endl;
        stbi_image_free(data);
        return 0;
    }
```

## Texture Setup

### Generate textureId

```cpp
    unsigned int textureID;
    glGenTextures(1, &textureID);
```

### Bind texture  and generate Mipmap in OpenGL

```cpp
    GLenum format;
    if (nrComponents == 1)
        format = GL_RED;
    else if (nrComponents == 3)
        format = GL_RGB;
    else if (nrComponents == 4)
        format = GL_RGBA;
    
    // We only use the default texture unit 0
    glActiveTexture(GL_TEXTURE0);
    glBindTexture(GL_TEXTURE_2D, textureID);
    glTexImage2D(GL_TEXTURE_2D, 0, format, width, height, 0, format, GL_UNSIGNED_BYTE, data);
    glGenerateMipmap(GL_TEXTURE_2D);
    
```

### Set OpenGL texture parameters&#x20;

```cpp
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
    
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
    
```

### Free image data and return textureid

```cpp
    stbi_image_free(data);
    
    return textureID;
}
```

## Full Source Code of Mesh::loadTextureAndBind()

```cpp
unsigned int Mesh::loadTextureAndBind(const char* path, const std::string& directory)
{
    std::string filename = std::string(path);
    filename = directory + '/' + filename;

    int width, height, nrComponents;
    unsigned char* data = stbi_load(filename.c_str(), &width, &height, &nrComponents, 0);
    if (! data)   {
        std::cout << "Texture failed to load at path: " << path << std::endl;
        stbi_image_free(data);
        return 0;
    }

    unsigned int textureID;
    glGenTextures(1, &textureID);

    GLenum format;
    if (nrComponents == 1)
        format = GL_RED;
    else if (nrComponents == 3)
        format = GL_RGB;
    else if (nrComponents == 4)
        format = GL_RGBA;

    // We only use the default texture unit 0
    glActiveTexture(GL_TEXTURE0);
    glBindTexture(GL_TEXTURE_2D, textureID);
    glTexImage2D(GL_TEXTURE_2D, 0, format, width, height, 0, format, GL_UNSIGNED_BYTE, data);
    glGenerateMipmap(GL_TEXTURE_2D);

    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

    stbi_image_free(data);

    return textureID;
}

```
