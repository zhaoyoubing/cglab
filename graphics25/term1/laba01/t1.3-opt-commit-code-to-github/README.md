---
description: This task is optional
---

# T1.3 \[opt] Commit code to Github

## Register a Github account

Before we start you need to register a Github account first.

## Write a .gitignore in your root directory

.gitignore specifies the files and directories will not commit by Git.

You can base your .gitignore on the visual studio templates if you use Visual Studio

{% embed url="https://github.com/github/gitignore/blob/main/VisualStudio.gitignore" %}

I appended the following to the Visual Studio template

```
# CMake
CMakeLists.txt.user
CMakeCache.txt
CMakeFiles
CMakeScripts
Testing
Makefile
cmake_install.cmake
install_manifest.txt
compile_commands.json
CTestTestfile.cmake
_deps
CMakeUserPresets.json
build/
out/

### VisualStudioCode ###
.vscode/*      # Maybe .vscode/**/* instead - see comments
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json

### Visual Studio Code Patch ###
# Ignore all local history of files
**/.history
```

##
