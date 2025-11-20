---
hidden: true
---

# Direct3D

## Drawing Triangles with Direct3D 12

### Objective

This tutorial introduces rendering triangles using Direct3D 12. We will set up a minimal Direct3D 12 application, render a single triangle, and extend it to indexed rendering.

### Prerequisites

* Windows 10 or later
* Visual Studio with Windows SDK
* Basic knowledge of C++

***

### Step 1: Setting Up the Direct3D 12 Environment

1. **Create a new Visual Studio project:** Choose an Empty Win32 Console Application.
2. **Include the required headers:**

```
#include <d3d12.h>
#include <dxgi1_6.h>
#include <d3dcompiler.h>
#include <DirectXMath.h>
#include <windows.h>
#include <wrl.h>
#include <iostream>
using namespace Microsoft::WRL;
```

3. **Link the necessary libraries:**
   * d3d12.lib
   * dxgi.lib
   * d3dcompiler.lib

***

### Step 2: Initialising Direct3D 12

1. **Create a window for rendering:**

```
LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam) {
    if (message == WM_DESTROY) {
        PostQuitMessage(0);
        return 0;
    }
    return DefWindowProc(hWnd, message, wParam, lParam);
}

HWND InitWindow(HINSTANCE hInstance) {
    WNDCLASS wc = {};
    wc.lpfnWndProc = WndProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = "Direct3D12Window";
    RegisterClass(&wc);
    return CreateWindow(wc.lpszClassName, "Direct3D 12 Tutorial", WS_OVERLAPPEDWINDOW,
                         CW_USEDEFAULT, CW_USEDEFAULT, 800, 600, nullptr, nullptr, hInstance, nullptr);
}
```

2. **Initialize Direct3D 12 components:**

```
ComPtr<ID3D12Device> device;
ComPtr<IDXGISwapChain3> swapChain;
ComPtr<ID3D12CommandQueue> commandQueue;
ComPtr<ID3D12DescriptorHeap> rtvHeap;
ComPtr<ID3D12Resource> renderTargets[2];
ComPtr<ID3D12CommandAllocator> commandAllocator;
ComPtr<ID3D12GraphicsCommandList> commandList;
ComPtr<ID3D12PipelineState> pipelineState;
ComPtr<ID3D12RootSignature> rootSignature;
UINT rtvDescriptorSize;

void InitD3D12(HWND hWnd) {
    ComPtr<IDXGIFactory4> factory;
    CreateDXGIFactory1(IID_PPV_ARGS(&factory));

    D3D12CreateDevice(nullptr, D3D_FEATURE_LEVEL_11_0, IID_PPV_ARGS(&device));

    D3D12_COMMAND_QUEUE_DESC queueDesc = {};
    queueDesc.Type = D3D12_COMMAND_LIST_TYPE_DIRECT;
    device->CreateCommandQueue(&queueDesc, IID_PPV_ARGS(&commandQueue));

    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {};
    swapChainDesc.BufferCount = 2;
    swapChainDesc.Width = 800;
    swapChainDesc.Height = 600;
    swapChainDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
    swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_DISCARD;
    swapChainDesc.SampleDesc.Count = 1;

    ComPtr<IDXGISwapChain1> tempSwapChain;
    factory->CreateSwapChainForHwnd(commandQueue.Get(), hWnd, &swapChainDesc, nullptr, nullptr, &tempSwapChain);
    tempSwapChain.As(&swapChain);
}
```

***

### Step 3: Rendering a Triangle

1. **Define a vertex structure:**

```
struct Vertex {
    DirectX::XMFLOAT3 position;
    DirectX::XMFLOAT4 color;
};
```

2. **Create a vertex buffer:**

```
Vertex vertices[] = {
    {{0.0f, 0.5f, 0.0f}, {1.0f, 0.0f, 0.0f, 1.0f}},
    {{0.5f, -0.5f, 0.0f}, {0.0f, 1.0f, 0.0f, 1.0f}},
    {{-0.5f, -0.5f, 0.0f}, {0.0f, 0.0f, 1.0f, 1.0f}}
};

ComPtr<ID3D12Resource> vertexBuffer;
D3D12_HEAP_PROPERTIES heapProps = {};
heapProps.Type = D3D12_HEAP_TYPE_UPLOAD;

D3D12_RESOURCE_DESC resDesc = {};
resDesc.Dimension = D3D12_RESOURCE_DIMENSION_BUFFER;
resDesc.Width = sizeof(vertices);
resDesc.Height = 1;
resDesc.DepthOrArraySize = 1;
resDesc.MipLevels = 1;
resDesc.SampleDesc.Count = 1;
resDesc.Layout = D3D12_TEXTURE_LAYOUT_ROW_MAJOR;

device->CreateCommittedResource(
    &heapProps, D3D12_HEAP_FLAG_NONE, &resDesc,
    D3D12_RESOURCE_STATE_GENERIC_READ, nullptr, IID_PPV_ARGS(&vertexBuffer));
```

3. **Render the triangle in the main loop:**

```
void Render() {
    // Clear the render target
    float clearColor[] = {0.0f, 0.0f, 0.0f, 1.0f};
    commandList->ClearRenderTargetView(rtvHeap->GetCPUDescriptorHandleForHeapStart(), clearColor, 0, nullptr);
    
    // Draw call
    commandList->IASetPrimitiveTopology(D3D_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    commandList->DrawInstanced(3, 1, 0, 0);
    
    swapChain->Present(1, 0);
}
```

***

### Step 4: Using Indexed Rendering

1. **Define an index buffer:**

```
UINT indices[] = {0, 1, 2};
ComPtr<ID3D12Resource> indexBuffer;
```

2. **Modify the render function to use indexed drawing:**

```
void Render() {
    commandList->IASetPrimitiveTopology(D3D_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    commandList->DrawIndexedInstanced(3, 1, 0, 0, 0);
    swapChain->Present(1, 0);
}
```

***

### Conclusion

This tutorial introduced rendering triangles in Direct3D 12, starting with a single triangle and moving to indexed rendering. Future steps include implementing shaders and handling user input.
