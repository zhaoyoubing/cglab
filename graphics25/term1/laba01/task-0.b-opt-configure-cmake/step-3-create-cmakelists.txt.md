# Step 3 Create CMakeLists.txt

## Create CMakeLists.txt

CMake is a cross-platform tool for project building, it uses CMakeLists.txt which specifies the include and library directories as well as your source code files.

Under the project root folder, such as proj1, create CMakeLists.txt

putting the following contents into the file, or download CMakeLists.txt from Blackboard

```cmake
cmake_minimum_required(VERSION 3.26.0)
project(proj01 VERSION 1.0.0)
cmake_policy(SET CMP0072 NEW)

# place for finding Findglfw3.cmake
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# external opengl related libraries 
find_package(OpenGL REQUIRED)
find_package(glfw3 REQUIRED)


# adding source files to our exectuable programs
# Note: it is not a good practice to put glad.c in the src folder
# Ideally, it should be put under external/ and used as an external library
# to avoid unnecessary compiling
add_executable(run01 src/main.cpp src/glad.c)

# specify include directories
target_include_directories(run01 PRIVATE include)

# specify library directories
target_link_libraries(run01 ${GLFW3_LIBRARY} OpenGL::GL)
```

## Install CMakeTools extension in VSCode

<figure><img src="../../../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

## Revise cmake/Findglfw3.cmake if you have a different external or include folder

Revise the CMAKE include and library directories of GLFW3 if you are putting GLFW in a different location other than "external" or are using a difference vrsion of visual studio

```cmake
# 5CM507 Note: revise the CMAKE incude directories if you are putting GLFW in a different location
set( _glfw3_HEADER_SEARCH_DIRS
"/usr/include"
"/usr/local/include"
"${CMAKE_SOURCE_DIR}/includes"
"${CMAKE_SOURCE_DIR}/external/glfw/include" )

# 5CM507 Note: revise the CMAKE library directories if you are putting GLFW in a different location
# or are using a difference vrsion of visual studio
set( _glfw3_LIB_SEARCH_DIRS
"/usr/lib"
"/usr/local/lib"
"${CMAKE_SOURCE_DIR}/external"
"${CMAKE_SOURCE_DIR}/external/glfw/lib-vc2022" )
```

## Test if CMake is working

Adding glad and GLFW headers in main.cpp&#x20;

Open the project with VSCode or Visual Studio (choose "Open Folder")

If the project compiles successful, CMake is working

```cpp
#include "glad/glad.h"
#include <GLFW/glfw3.h>
```

