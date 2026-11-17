# Vulkan Engine

A single-file C++ Vulkan learning project following the [Vulkan Tutorial](https://vulkan-tutorial.com) by Alexander Overvoorde.

## Dependencies

- CMake 3.16 or newer
- A C++17 compiler
- Vulkan SDK, including `glslc`
- GLFW 3
- GLM

## Build and run

Configure and build from the project root:

```bash
cmake -S . -B build
cmake --build build
./build/VulkanEngine
```

CMake compiles the GLSL files in `shaders/` to SPIR-V in the build directory. The executable loads those shaders and the model and texture from paths based on the project layout, so it can be launched from any working directory.

## Project layout

- `src/main.cpp` contains the Vulkan application.
- `include/` contains the header-only image and OBJ-loading libraries.
- `shaders/` contains the GLSL shader sources and precompiled SPIR-V files.
- `resource files/objects/` contains the OBJ model.
- `resource files/textures/` contains the model texture and other image assets.

## Current features

- Creates a Vulkan instance and enables the validation layer in debug builds.
- Selects a device with graphics and presentation queues and swapchain support.
- Creates a swapchain, render pass, graphics pipeline, and depth buffer.
- Loads GLSL shaders as compiled SPIR-V at runtime.
- Uses staging buffers, uniform buffers, descriptor sets, and a sampled texture.
- Loads and deduplicates OBJ model vertices.
- Updates model, view, and projection matrices through uniform buffers.
