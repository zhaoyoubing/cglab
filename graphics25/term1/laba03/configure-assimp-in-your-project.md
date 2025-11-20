# Configure Assimp in your project

Assimp is the library we are going to use to load our 3D models.

I have downloaded and installed Assimp in the project, but it only works for VS2022 compiler.

Start your project from here:

{% embed url="https://github.com/zhaoyoubing/proj02/releases/tag/Lab3_initial_assimp" %}

If you need to configure it by yourself, you need to download the source code and compile it.&#x20;

After that copy the libraries to the external folder and header directory to the include folder.

You also need to change CMakefile.Lists to set the directories. ( You can follow mine despite that it is only a workaround).
