cmake_minimum_required(VERSION 3.16)
project(imgui)

set(CMAKE_CXX_STANDARD 17)

# All available rendering backend options.
# Set the ones that you use to TRUE, either here (by uncommenting the necessary lines)
# or in another CMakeLists.txt file, BEFORE including this file.

# set(IMGUI_BACKEND_GLFW TRUE)
# set(IMGUI_BACKEND_Vulkan TRUE)
# set(IMGUI_BACKEND_OpenGL2 TRUE)
# set(IMGUI_BACKEND_OpenGL3 TRUE)

# When using GLFW, you have to additionally specify the path to its include folder.
# Note that we start from the location of this CMakeLists.txt file.
# set(IMGUI_BACKEND_GLFW_PATH "../glfw/include")

# ImGui source files
set(IMGUI_SOURCES
        imconfig.h
        imgui.h
        imgui.cpp
        imgui_demo.cpp
        imgui_draw.cpp
        imgui_internal.h
        imgui_tables.cpp
        imgui_widgets.cpp
        imstb_rectpack.h
        imstb_textedit.h
        imstb_truetype.h)

# Add GLFW source files
if (IMGUI_BACKEND_GLFW)
    list(APPEND IMGUI_SOURCES "backends/imgui_impl_glfw.h" "backends/imgui_impl_glfw.cpp")
endif ()

# Require Vulkan and include source files
if (IMGUI_BACKEND_Vulkan)
    find_package(Vulkan REQUIRED FATAL_ERROR)
    list(APPEND IMGUI_SOURCES "backends/imgui_impl_vulkan.h" "backends/imgui_impl_vulkan.cpp")
endif ()

# Add OpenGL2 source files
if (IMGUI_BACKEND_OpenGL2)
    list(APPEND IMGUI_SOURCES "backends/imgui_impl_opengl2.h" "backends/imgui_impl_opengl2.cpp")
endif ()

# Add OpenGL3 source files
if (IMGUI_BACKEND_OpenGL3)
    list(APPEND IMGUI_SOURCES "backends/imgui_impl_opengl3.h" "backends/imgui_impl_opengl3.cpp")
endif ()

# Create static library with specified files
add_library(imgui STATIC "${IMGUI_SOURCES}")

# Include all directories starting from the root
target_include_directories(imgui PUBLIC ./)

# Add GLFW directory
if (IMGUI_BACKEND_GLFW)
    target_include_directories(imgui PUBLIC "${IMGUI_BACKEND_GLFW_PATH}")
endif ()

# Include and link Vulkan
if (IMGUI_BACKEND_Vulkan)
    target_include_directories(imgui PUBLIC ${Vulkan_INCLUDE_DIR})
    target_link_libraries(imgui ${Vulkan_LIBRARIES})
endif ()
