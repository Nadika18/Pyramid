cmake_minimum_required(VERSION 3.0)
project(ClassroomModel)
add_executable(${PROJECT_NAME} src/main.cpp)
add_subdirectory(external_libraries/glfw)
target_include_directories(${PROJECT_NAME}
    PUBLIC external_libraries/glfw/include
)
target_link_directories(${PROJECT_NAME}
    PRIVATE external_libraries/glfw/src
)
target_link_libraries(${PROJECT_NAME} glfw)