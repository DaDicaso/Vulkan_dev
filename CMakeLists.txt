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

function(add_slang_shader_target TARGET)
    cmake_parse_arguments("SHADER" "" "" "SOURCES" ${ARGN})
    set (SHADERS_DIR ${CMAKE_CURRENT_LIST_DIR}/shaders)
    set (ENTRY_POINTS -entry vertMain -entry fragMain)
    add_custom_command(
            OUTPUT $(SHADERS_DIR)
            COMMAND $(CMAKE_COMMAND) -E make_directory ${SHADER_DIR}
    )
    add_custom_command(
            OUTPUT ${SHADER_DIR}/slang.spv
            COMMAND $(SLANGC_EXECUTABLE) ${SHADER_SOURCES} -target spirv -profile spirv_1_4 -emit-spirv-directory -fvk-use-entrypoint-name ${ENTRY_POINTS} -o shaders/slang.spv
            WORKING_DIRECTORY ${SHADERS_DIR}
            DEPENDS ${SHADERS_DIR} ${SHADER_SOURCES}
            COMMENT "Compiling Slang Shaders"
            VERBATIM
    )
    add_custom_target(${TARGET} DEPENDS ${SHADERS_DIR}/slang.spv)
endfunction()

add_slang_shader_target(foo SOURCES ${SHADER_SLANG_SOURCES})
add_dependencies(VulkanTriangle foo)

