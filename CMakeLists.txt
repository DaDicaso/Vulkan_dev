cmake_minimum_required(VERSION 3.22)
project(VulkanTriangle LANGUAGES CXX)

# Use modern C++20 (highly recommended for Vulkan's designated initializers)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 1. Locate the Vulkan SDK on your system
find_package(Vulkan REQUIRED)

# 2. Locate GLFW (Windowing Library)
find_package(glfw3 CONFIG)
if(NOT glfw3_FOUND)
    # Fallback to standard package check if config is not found
    find_package(GLFW REQUIRED)
endif()

# 3. Define the executable and add your source files
add_executable(VulkanTriangle main.cpp)

# 4. Bind the header include directories
target_include_directories(VulkanTriangle PRIVATE 
    ${Vulkan_INCLUDE_DIRS}
)

# 5. Link the Vulkan and GLFW libraries to your application
target_link_libraries(VulkanTriangle PRIVATE 
    Vulkan::Vulkan 
    glfw
)
