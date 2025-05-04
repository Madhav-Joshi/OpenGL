cmake_minimum_required(VERSION 3.10)

project(Glad LANGUAGES C)

# Create a static library
add_library(Glad STATIC
    src/glad.c
    include/glad/glad.h
    include/KHR/khrplatform.h
)

# Set the include directories
target_include_directories(Glad PUBLIC include)

# Set the output directories for the library
set_target_properties(Glad PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/Glad"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/Glad"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/Glad"
    OUTPUT_NAME "Glad"
)

# Set the object file directory
set_property(TARGET Glad PROPERTY
    OBJECT_OUTPUTS "${CMAKE_BINARY_DIR}/bin-int/${CMAKE_BUILD_TYPE}/Glad"
)

# Compiler settings for Windows
if(WIN32)
    target_compile_definitions(Glad PRIVATE _CRT_SECURE_NO_WARNINGS)
endif()

# Configuration-specific settings
target_compile_options(Glad PRIVATE
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O3>
)