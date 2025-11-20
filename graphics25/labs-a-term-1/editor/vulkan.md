---
hidden: true
---

# Vulkan

## Lab Tutorial: Drawing Triangles with Vulkan

### Objective

This tutorial introduces rendering triangles using Vulkan. We will set up a minimal Vulkan application, render a single triangle, and extend it to indexed rendering.

### Prerequisites

* Windows or Linux operating system
* Vulkan SDK installed
* Visual Studio (Windows) or a GCC-compatible compiler (Linux)
* Basic knowledge of C++

***

### Step 1: Setting Up the Vulkan Environment

1. **Create a new project**
   * For Windows, use an Empty Console Application in Visual Studio.
   * For Linux, create a new C++ project with CMake.
2. **Include Vulkan headers:**

```
#include <vulkan/vulkan.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <vector>
```

3. **Link Vulkan libraries:**
   * Windows: `vulkan-1.lib`
   * Linux: `-lvulkan`
4. **Initialize GLFW for window creation:**

```
if (!glfwInit()) {
    std::cerr << "Failed to initialize GLFW!" << std::endl;
    return -1;
}

glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);
GLFWwindow* window = glfwCreateWindow(800, 600, "Vulkan Window", nullptr, nullptr);
```

***

### Step 2: Initialising Vulkan

1. **Create a Vulkan instance:**

```
VkInstance instance;
VkApplicationInfo appInfo = {};
appInfo.sType = VK_STRUCTURE_TYPE_APPLICATION_INFO;
appInfo.pApplicationName = "Vulkan Triangle";
appInfo.applicationVersion = VK_MAKE_VERSION(1, 0, 0);
appInfo.pEngineName = "No Engine";
appInfo.engineVersion = VK_MAKE_VERSION(1, 0, 0);
appInfo.apiVersion = VK_API_VERSION_1_0;

VkInstanceCreateInfo createInfo = {};
createInfo.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
createInfo.pApplicationInfo = &appInfo;

target VkResult result = vkCreateInstance(&createInfo, nullptr, &instance);
if (result != VK_SUCCESS) {
    std::cerr << "Failed to create Vulkan instance!" << std::endl;
    return -1;
}
```

2. **Select a physical device (GPU):**

```
uint32_t deviceCount = 0;
vkEnumeratePhysicalDevices(instance, &deviceCount, nullptr);
std::vector<VkPhysicalDevice> devices(deviceCount);
vkEnumeratePhysicalDevices(instance, &deviceCount, devices.data());
VkPhysicalDevice physicalDevice = devices[0];
```

3. **Create a logical device:**

```
VkDevice device;
VkDeviceCreateInfo deviceCreateInfo = {};
deviceCreateInfo.sType = VK_STRUCTURE_TYPE_DEVICE_CREATE_INFO;
vkCreateDevice(physicalDevice, &deviceCreateInfo, nullptr, &device);
```

***

### Step 3: Rendering a Triangle

1. **Define a vertex structure:**

```
struct Vertex {
    float position[2];
    float color[3];
};
```

2. **Create a vertex buffer:**

```
Vertex vertices[] = {
    {{0.0f, -0.5f}, {1.0f, 0.0f, 0.0f}},
    {{0.5f, 0.5f}, {0.0f, 1.0f, 0.0f}},
    {{-0.5f, 0.5f}, {0.0f, 0.0f, 1.0f}}
};

VkBuffer vertexBuffer;
VkDeviceMemory vertexBufferMemory;
```

3. **Create a Vulkan pipeline and render the triangle:**

```
void Render() {
    VkCommandBuffer commandBuffer;
    VkCommandBufferBeginInfo beginInfo = {};
    beginInfo.sType = VK_STRUCTURE_TYPE_COMMAND_BUFFER_BEGIN_INFO;
    vkBeginCommandBuffer(commandBuffer, &beginInfo);

    vkCmdBindVertexBuffers(commandBuffer, 0, 1, &vertexBuffer, 0);
    vkCmdDraw(commandBuffer, 3, 1, 0, 0);

    vkEndCommandBuffer(commandBuffer);
}
```

***

### Step 4: Using Indexed Rendering

1. **Define an index buffer:**

```
uint16_t indices[] = {0, 1, 2};
VkBuffer indexBuffer;
VkDeviceMemory indexBufferMemory;
```

2. **Modify the render function to use indexed drawing:**

```
void Render() {
    VkCommandBuffer commandBuffer;
    VkCommandBufferBeginInfo beginInfo = {};
    beginInfo.sType = VK_STRUCTURE_TYPE_COMMAND_BUFFER_BEGIN_INFO;
    vkBeginCommandBuffer(commandBuffer, &beginInfo);

    vkCmdBindVertexBuffers(commandBuffer, 0, 1, &vertexBuffer, 0);
    vkCmdBindIndexBuffer(commandBuffer, indexBuffer, 0, VK_INDEX_TYPE_UINT16);
    vkCmdDrawIndexed(commandBuffer, 3, 1, 0, 0, 0);

    vkEndCommandBuffer(commandBuffer);
}
```

***

### Conclusion

This tutorial introduced rendering triangles in Vulkan, starting with a single triangle and moving to indexed rendering. Future steps include implementing shaders, handling user input, and optimizing performance.
