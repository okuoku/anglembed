cmake_minimum_required(VERSION 3.20)
project(anglembed)

set(deps ${CMAKE_CURRENT_LIST_DIR}/deps)
set(root ${CMAKE_CURRENT_LIST_DIR})

# glslang and SPIRV-Tools

set(SPIRV_WERROR OFF CACHE BOOL "" FORCE)
set(SPIRV-Headers_SOURCE_DIR ${deps}/SPIRV-Headers)
set(SKIP_GLSLANG_INSTALL OFF CACHE BOOL "" FORCE)
set(ENABLE_GLSLANG_BINARIES OFF CACHE BOOL "" FORCE)
set(SKIP_SPIRV_TOOLS_INSTALL OFF CACHE BOOL "" FORCE)
set(SPIRV_SKIP_EXECUTABLES ON CACHE BOOL "" FORCE)

add_subdirectory(${deps}/SPIRV-Tools SPIRV-Tools)
add_subdirectory(${deps}/glslang glslang)

# Workaround for glslang-default-resource-limits
set(glslang-resource-limits-srcs
    ${deps}/glslang/glslang/ResourceLimits/ResourceLimits.cpp
    ${deps}/glslang/glslang/ResourceLimits/resource_limits_c.cpp)
include_directories(${deps}/glslang)
add_library(my-glslang-default-resource-limits STATIC
    ${glslang-resource-limits-srcs})

# Angle

set(ANGLE_SPIRV_HEADERS_ROOT ${deps}/SPIRV-Headers)
set(ANGLE_VULKAN_HEADERS_ROOT ${deps}/Vulkan-Headers)
set(ANGLE_SPIRV_TOOLS_ROOT ${deps}/SPIRV-Tools)
set(ANGLE_VOLK_ROOT ${deps}/volk) # Vulkan only?
set(ANGLE_VMA_ROOT ${deps}/VulkanMemoryAllocator) # Vulkan only?
set(ANGLE_GLSLANG_ROOT ${deps}/glslang)
set(ANGLE_SPIRV_CROSS_ROOT ${deps}/SPIRV-Cross)
set(ANGLE_BACKEND Vulkan)

set(ANGLE_ROOT ${root}/angle)
set(CHROMIUM_ZLIB_ROOT ${deps}/chromium-zlib) # Move this?

add_subdirectory(${root}/angle-static-cmake angle)
