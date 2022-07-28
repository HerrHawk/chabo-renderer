cmake_minimum_required(VERSION 3.20.3)
project (chabo)

set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /SUBSYSTEM:WINDOWS /ENTRY:mainCRTStartup")

file(GLOB SOURCES "chabo/src/*.cpp")
add_executable(${CMAKE_PROJECT_NAME} WIN32 ${SOURCES})


# OpenGL
find_package(OpenGL REQUIRED)
# GLFW
find_library(GLFW glfw3 PATHS "external/GLFW/lib-vc2022")
include_directories(external/GLFW/include)
# glad
add_library(glad "external/glad/src/glad.c")
include_directories(external/glad/include)

# Imgui
set(IMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR}/external/imgui)
target_sources( ${CMAKE_PROJECT_NAME}
                PRIVATE
                    ${IMGUI_DIR}/imgui_demo.cpp
                    ${IMGUI_DIR}/imgui_draw.cpp
                    ${IMGUI_DIR}/imgui_tables.cpp
                    ${IMGUI_DIR}/imgui_widgets.cpp
                    ${IMGUI_DIR}/imgui.cpp

                PRIVATE
                    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
                    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
                )
target_include_directories( ${CMAKE_PROJECT_NAME}
                            PUBLIC chabo/include
                            PUBLIC ${IMGUI_DIR}
                            PUBLIC ${IMGUI_DIR}/backends
                            PUBLIC ${SDL2_DIR}/include
                            PUBLIC external/glad/include
                            )



target_link_libraries(${CMAKE_PROJECT_NAME}
    ${GLFW}
    ${glad}
    ${OPENGL_gl_LIBRARY}
)



