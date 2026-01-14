cmake_minimum_required(VERSION 3.10)
project(IKRT)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set policy to suppress OpenGL warning
if(POLICY CMP0072)
    cmake_policy(SET CMP0072 NEW)
endif()

# --- Find required packages (Original Setup) ---
find_package(OpenGL REQUIRED)
find_package(glm REQUIRED)
find_package(GLEW REQUIRED)
find_package(glfw3 REQUIRED)
find_package(assimp REQUIRED) # <-- KEPT EXACTLY AS YOU HAD IT


# --- Dear ImGui Integration ---
# 1. Define the location of the ImGui source files
set(IMGUI_DIR ${CMAKE_SOURCE_DIR}/third_party/imgui)

# 2. Create a static library for ImGui
add_library(imgui STATIC
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/imgui_impl_opengl3.cpp
    ${IMGUI_DIR}/ImGuiFileDialog.cpp
    ${IMGUI_DIR}/ImGuizmo.cpp
    ${IMGUI_DIR}/ImSequencer.cpp
)

# 3. Provide include directories for ImGui
target_include_directories(imgui PUBLIC
    ${IMGUI_DIR}
)

# 4. Link ImGui to its own dependencies (GLFW and OpenGL)
target_link_libraries(imgui PUBLIC
    glfw
    ${OPENGL_LIBRARIES}
)


# --- Your Application ---

# Create executable
add_executable(NoCap source/main.cpp)

# Modern CMake: Use target-specific include directories
target_include_directories(NoCap PRIVATE
    ${CMAKE_SOURCE_DIR}/header
    ${CMAKE_SOURCE_DIR}/third_party/stb_image
    ${CMAKE_SOURCE_DIR}/third_party/nlohmann
    ${CMAKE_SOURCE_DIR}/third_party/assimp/include
)

# Link libraries
target_link_libraries(NoCap PRIVATE
    imgui
    OpenGL::GL
    glm::glm
    GLEW::GLEW
    glfw
    assimp
)

# Set the runtime working directory to the project root
set_target_properties(NoCap PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR})