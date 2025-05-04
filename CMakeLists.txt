cmake_minimum_required(VERSION 3.10)

# OpenGL-Sandbox Workspace
project(OpenGL-Sandbox LANGUAGES CXX)

# Set the output directory for build configurations
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin-int)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Include directories relative to OpenGL-Core
set(IncludeDir_GLFW "${CMAKE_SOURCE_DIR}/OpenGL-Core/vendor/GLFW/include")
set(IncludeDir_Glad "${CMAKE_SOURCE_DIR}/OpenGL-Core/vendor/Glad/include")
set(IncludeDir_ImGui "${CMAKE_SOURCE_DIR}/OpenGL-Core/vendor/imgui")
set(IncludeDir_glm "${CMAKE_SOURCE_DIR}/OpenGL-Core/vendor/glm")
set(IncludeDir_stb_image "${CMAKE_SOURCE_DIR}/OpenGL-Core/vendor/stb_image")

# Add subdirectories for dependencies
add_subdirectory(OpenGL-Core/vendor/GLFW)
add_subdirectory(OpenGL-Core/vendor/Glad)
add_subdirectory(OpenGL-Core/vendor/imgui)

# Add the OpenGL-Core project
add_subdirectory(OpenGL-Core)

# Add the OpenGL-Sandbox project
add_subdirectory(OpenGL-Sandbox)

# OpenGL-Examples Workspace
project(OpenGL-Examples LANGUAGES CXX)

# Add the OpenGL-Examples project
add_subdirectory(OpenGL-Examples)