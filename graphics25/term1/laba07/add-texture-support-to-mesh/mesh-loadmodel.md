# Mesh::loadModel()

## Change Mesh::loadModel()

Revisions in  loadModel()&#x20;

1. to load model with flipped texture image

```cpp
const aiScene* scene = importer.ReadFile(path, aiProcess_JoinIdenticalVertices | aiProcess_FlipUVs);
```

2. use the new vertex list with struct Vertex

```cpp
    // LabA07
    Vertex v;

    // at the moment we only handle one mesh
    for (int i = 0; i < scene->mNumMeshes; i++)
    {
        aiMesh* mesh = scene->mMeshes[i];
        
        // read vertex position and normals
        int nVertex = mesh->mNumVertices;
        // std::cout << mesh->mNumVertices << std::endl;
        //std::cout << "texCoord length: " << mesh->mTextureCoords[0]->Length() << std::endl;

        // changed in LabA07
        for (int j = 0; j < nVertex; j++)
        {
            glm::vec3 pos; 
            pos.x = mesh->mVertices[j].x;
            pos.y = mesh->mVertices[j].y;
            pos.z = mesh->mVertices[j].z; 
            // vertices.push_back(pos);
            v.pos = pos;

            glm::vec3 normal;
            normal.x = mesh->mNormals[j].x;
            normal.y = mesh->mNormals[j].y;
            normal.z = mesh->mNormals[j].z;
            //vertices.push_back(normal);
            v.normal = normal;

            // LabA07 Texture Coordinates
            if(mesh->mTextureCoords[0]) // does the mesh contain texture coordinates?
            {
                glm::vec2 vec;
                vec.x = mesh->mTextureCoords[0][j].x; 
                vec.y = mesh->mTextureCoords[0][j].y;
                v.texCoord = vec;
            }
            else {
                v.texCoord = glm::vec2(0.0f, 0.0f);
            }
            vertices.push_back(v);
        }

        // unchanged
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
```

3. Load textures

Warning: we only support a Mesh with one material/texture

```cpp
    // added in LabA07
    // loading material and texture, at the moment we only deal with one material/texture
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
    }
```
