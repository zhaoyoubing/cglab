---
description: Mesh.cpp
---

# Load Normal map and tangent space

Now we are moving to Mesh.cpp

We need to revise Mesh::loadModel() to create the tangent space and load the tangent/bitangent together with the normal map.

## Create the tangent space

Assimp supports creation of the tangent space with the option **aiProcess\_CalcTangentSpace**.

```cpp
void Mesh::loadModel(std::string path) 
{
    Assimp::Importer importer;
    const aiScene* scene = importer.ReadFile(path, aiProcess_JoinIdenticalVertices 
                                       | aiProcess_FlipUVs 
                                       | aiProcess_CalcTangentSpace );
    
    ...
}
```

## Load the tangent/bitangent

Then we can read the tangents from the data

```cpp
for (int i = 0; i < scene->mNumMeshes; i++)
    {
        aiMesh* mesh = scene->mMeshes[i];
        
        // read vertex position and normals
        int nVertex = mesh->mNumVertices;

        for (int j = 0; j < nVertex; j++)
        {
            glm::vec3 pos; 
            ... read pos
            v.pos = pos;

            glm::vec3 normal;
            ... read normal
            v.normal = normal;

            // LabA08 read tangents for normal mapping
            glm::vec3 tangent;
            tangent.x = mesh->mTangents[i].x;
            tangent.y = mesh->mTangents[i].y;
            tangent.z = mesh->mTangents[i].z;
            v.tangent = tangent;  

            tangent.x = mesh->mBitangents[i].x;
            tangent.y = mesh->mBitangents[i].y;
            tangent.z = mesh->mBitangents[i].z;
            v.bitangent = tangent; 
            
            ...
            }
    }

```

## Load the normal map

In addition to the diffuse texture map, now we need to load the normal map.

After loading the texture map, add loading of the normal map.

Because the OBJ material does not has a field for normal map, actually we are using the map\_Bump option (change of height) for the normal map, as can be seen from Box\_normal.mtl.

```
newmtl None
kd 0.8 0.8 0.8
ka 0.8 0.8 0.8
ks 0.8 0.8 0.8
ke 0 0 0
d 1
Ns 2000
illum 2
map_Kd Box_BaseColor.png
map_Bump Box_Normal.png
```



So the corresponding texture type for the normal map is **aiTextureType\_HEIGHT**.

```cpp
    aiMesh* mesh = scene->mMeshes[0];

    if (NULL != mesh && mesh->mMaterialIndex > 0)
    {
       ...
       
        aiMaterial* material = scene->mMaterials[mesh->mMaterialIndex];
        std::vector<Texture> diffuseMaps = loadMaterialTextures(material,
            aiTextureType_DIFFUSE, "texture_diffuse", dir);
        textures.insert(textures.end(), diffuseMaps.begin(), diffuseMaps.end());

        std::vector<Texture> normalMaps = loadMaterialTextures(material,
            aiTextureType_HEIGHT, "texture_normal", dir);
        normals.insert(normals.end(), normalMaps.begin(), normalMaps.end());

    }
```

## Full Source Code of Mesh::loadModel()

```cpp
void Mesh::loadModel(std::string path) 
{
    Assimp::Importer importer;
    const aiScene* scene = importer.ReadFile(path, aiProcess_JoinIdenticalVertices | aiProcess_FlipUVs | aiProcess_CalcTangentSpace /* | aiProcess_GenNormals */ );
    if (NULL != scene) {
        std::cout << "load model successful" << std::endl;
    } else {
        std::cout << "load model failed" << std::endl;
    }

    Vertex v;

    // at the moment we only handle one mesh
    for (int i = 0; i < scene->mNumMeshes; i++)
    {
        aiMesh* mesh = scene->mMeshes[i];
        
        // read vertex position and normals
        int nVertex = mesh->mNumVertices;

        for (int j = 0; j < nVertex; j++)
        {
            glm::vec3 pos; 
            pos.x = mesh->mVertices[j].x;
            pos.y = mesh->mVertices[j].y;
            pos.z = mesh->mVertices[j].z; 
            v.pos = pos;

            glm::vec3 normal;
            normal.x = mesh->mNormals[j].x;
            normal.y = mesh->mNormals[j].y;
            normal.z = mesh->mNormals[j].z;
            v.normal = normal;

            // LabA08 tangent space for normal mapping
            glm::vec3 tangent;
            tangent.x = mesh->mTangents[i].x;
            tangent.y = mesh->mTangents[i].y;
            tangent.z = mesh->mTangents[i].z;
            v.tangent = tangent;  

            tangent.x = mesh->mBitangents[i].x;
            tangent.y = mesh->mBitangents[i].y;
            tangent.z = mesh->mBitangents[i].z;
            v.bitangent = tangent; 

            // LabA07 Texture Coordinates
            if(mesh->mTextureCoords[0]) // does the mesh contain texture coordinates?
            {
                glm::vec2 vec;
                vec.x = mesh->mTextureCoords[0][j].x; 
                vec.y = mesh->mTextureCoords[0][j].y;
                v.texCoord = vec;
            }
            else {
                std::cout << "tex coord zero" << std::endl;
                v.texCoord = glm::vec2(0.0f, 0.0f);
            }
            vertices.push_back(v);
        }

	int nFaces = mesh->mNumFaces;
        for (int j = 0; j < nFaces; j++ )
        {
            const aiFace& face = mesh->mFaces[j];
            for (int k = 0; k < 3; k++)
            {
                indices.push_back(face.mIndices[k]); 
            }
        }
    }

    // at the moment
    // we only deal with one material/texture
    aiMesh* mesh = scene->mMeshes[0];

    if (NULL != mesh && mesh->mMaterialIndex > 0)
    {
        std::string dir = "";
        const size_t last_slash_idx = path.rfind('/');
        if (std::string::npos != last_slash_idx)
        {
            dir = path.substr(0, last_slash_idx);
        }

        aiMaterial* material = scene->mMaterials[mesh->mMaterialIndex];
        std::vector<Texture> diffuseMaps = loadMaterialTextures(material,
            aiTextureType_DIFFUSE, "texture_diffuse", dir);
        textures.insert(textures.end(), diffuseMaps.begin(), diffuseMaps.end());

        std::vector<Texture> normalMaps = loadMaterialTextures(material,
            aiTextureType_HEIGHT, "texture_normal", dir);
        normals.insert(normals.end(), normalMaps.begin(), normalMaps.end());

    }

}
```
