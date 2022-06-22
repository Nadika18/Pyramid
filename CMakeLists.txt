cmake_minimum_required(VERSION 3.0)
project(ClassroomModel)
set(OpenGL_GL_PREFERENCE GLVND)
find_package(OpenGL REQUIRED)
add_executable(main src/main.cpp)
add_subdirectory(external_libraries/glfw)
add_subdirectory(external_libraries/glm)
find_package(GLUT REQUIRED)
find_package(GLU REQUIRED)
target_include_directories(main
    PUBLIC external_libraries/glfw/include

)
target_link_directories(main
    PRIVATE external_libraries/glfw/src

    

)
target_link_libraries(main glfw ${OPENGL_LIBRARY} glm glut GLU GL)

