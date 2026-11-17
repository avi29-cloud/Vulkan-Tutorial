cmake_minimum_required(VERSION 3.16)

project(VulkanEngine LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(Vulkan REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)
find_program(GLSLC_EXECUTABLE NAMES glslc)

if(NOT GLSLC_EXECUTABLE)
    message(FATAL_ERROR "glslc was not found. Install the Vulkan SDK or add glslc to PATH.")
endif()

set(SHADER_OUTPUT_DIR "${CMAKE_CURRENT_BINARY_DIR}/shaders")
set(VERT_SHADER_OUTPUT "${SHADER_OUTPUT_DIR}/vert.spv")
set(FRAG_SHADER_OUTPUT "${SHADER_OUTPUT_DIR}/frag.spv")

add_custom_command(
    OUTPUT "${VERT_SHADER_OUTPUT}"
    COMMAND "${CMAKE_COMMAND}" -E make_directory "${SHADER_OUTPUT_DIR}"
    COMMAND "${GLSLC_EXECUTABLE}" "${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert" -o "${VERT_SHADER_OUTPUT}"
    DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert"
    VERBATIM
)

add_custom_command(
    OUTPUT "${FRAG_SHADER_OUTPUT}"
    COMMAND "${CMAKE_COMMAND}" -E make_directory "${SHADER_OUTPUT_DIR}"
    COMMAND "${GLSLC_EXECUTABLE}" "${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag" -o "${FRAG_SHADER_OUTPUT}"
    DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag"
    VERBATIM
)

add_custom_target(VulkanEngineShaders DEPENDS "${VERT_SHADER_OUTPUT}" "${FRAG_SHADER_OUTPUT}")

add_executable(VulkanEngine src/main.cpp)
add_dependencies(VulkanEngine VulkanEngineShaders)

target_include_directories(VulkanEngine PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/include")
target_compile_definitions(VulkanEngine PRIVATE
    VULKAN_ENGINE_SHADER_DIR="${SHADER_OUTPUT_DIR}"
    VULKAN_ENGINE_RESOURCE_DIR="${CMAKE_CURRENT_SOURCE_DIR}/resource files"
)
target_link_libraries(VulkanEngine PRIVATE Vulkan::Vulkan glfw glm::glm)
