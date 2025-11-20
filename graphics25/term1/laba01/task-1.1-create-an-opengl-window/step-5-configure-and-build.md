# Step 5 Configure and Build

## VSCode

Use the CMake Tools sidebar to configure, build and run your project is most convenient

## Configure

Click on the configure icon to configure your project.

<figure><img src="../../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

You may be asked to select your compiler and architecture. In this example we are using Visual Studio 2022 on a 64 bit platform.

You can also select to set the build version to be Debug or Release.

### Build

Click on the Build icon, it will start compiling your project. If successful, the exit code should be 0.

<figure><img src="../../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

### After building

After building, a build directory appears.

If building is successful, you can see run01.exe in your Debug or Release directory.

<figure><img src="../../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### Alternatively, you can use the Command Pallette

Select View->Command Pallette

Type CMake:Build

<figure><img src="../../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

Select your own compiler

I am using  Visual Studio 2022 with an x64 architecture, so I select Visual Studio 2022 with amd64, or you can choose "Scan for kits"

<figure><img src="../../../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

###

## Visual Studio

Visual Studio supports CMake as well.&#x20;

You can use Visual Studio to directly open the project folder.

When you choose Build->Build All, it will create an out folder and putting all intermediate files in out/build

<figure><img src="../../../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

## CMake-GUI

Instead of the CMake extention in VSCodde, you can also use CMake-GUI to generate your Visual Studio project file.

Use CMake-GUI to open your project folder and specify your build folder.

After clicking on the Configure button, you can select your compiler.

<figure><img src="../../../../.gitbook/assets/8e744026-ff17-4530-b023-5c772286fbbc.png" alt=""><figcaption></figcaption></figure>

Clicking Configure and Generate, will generate the Visual Studio project file

<figure><img src="../../../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/ed093bd6-0e40-4e9a-905a-e6401fab617f.png" alt=""><figcaption></figcaption></figure>

The project files created are similar to what you have seen in VSCode. You can use Visual Studio to directly open the solution file.

From the project properties, you can see that the include directories, OpenGL and GLFW libraries are all added.

<figure><img src="../../../../.gitbook/assets/acf784e9-252e-4b77-b2c9-02d1b224a79a.png" alt=""><figcaption></figcaption></figure>
