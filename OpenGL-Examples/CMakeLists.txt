cmake_minimum_required(VERSION 3.10)

project(OpenGL-Examples LANGUAGES CXX)

# Set the C++ standard and static runtime
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

# Add the executable target
file(GLOB_RECURSE SRC_FILES "src/*.cpp" "src/*.h")

add_executable(OpenGL-Examples
    ${SRC_FILES}
)

# Include directories
target_include_directories(OpenGL-Examples PUBLIC
    ../OpenGL-Core/vendor/spdlog/include
    ../OpenGL-Core/src
    ../OpenGL-Core/vendor
    ../OpenGL-Core/${IncludeDir_glm}
    ../OpenGL-Core/${IncludeDir_Glad}
    ../OpenGL-Core/${IncludeDir_ImGui}
)

# Link libraries
target_link_libraries(OpenGL-Examples PRIVATE
    OpenGL-Core
)

# Platform-specific settings
if(WIN32)
    target_compile_definitions(OpenGL-Examples PRIVATE
        GLCORE_PLATFORM_WINDOWS
    )
endif()

# Configuration-specific settings
target_compile_definitions(OpenGL-Examples PRIVATE
    $<$<CONFIG:Debug>:GLCORE_DEBUG>
    $<$<CONFIG:Release>:GLCORE_RELEASE>
)

target_compile_options(OpenGL-Examples PRIVATE
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O3>
)

# Output directories
set_target_properties(OpenGL-Examples PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Examples"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Examples"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Examples"
)

# Copy assets directory to the binary build directory
add_custom_command(TARGET OpenGL-Examples POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    "${CMAKE_CURRENT_SOURCE_DIR}/assets"
    "$<TARGET_FILE_DIR:OpenGL-Examples>/assets"
)