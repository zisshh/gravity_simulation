cmake_minimum_required(VERSION 3.20...3.27)
project(gravitysimul VERSION 1.0.0)
find_package(OpenGL REQUIRED)
find_package(glm REQUIRED)
add_executable(gravitysimul
    src/config.h
    src/glad.c
    src/finalgravitysim.cpp
)

target_include_directories(gravitysimul PRIVATE 
    dependencies
    /usr/local/include
    /opt/homebrew/include
)

target_link_libraries(gravitysimul glfw OpenGL::GL)