cmake_minimum_required(VERSION 3.0)


project(ClassroomModel)

configure_file(config/rootDirectory.h.in config/rootDirectory.h)

if(MSVC AND NOT "${MSVC_VERSION}" LESS 1400)
	add_definitions( "/MP" )
endif()



if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
set(OPENGL_LIBRARY
	${OPENGL_LIBRARY}
	-lGL -lGLU -lXrandr -lXext -lX11 -lrt
	${CMAKE_DL_LIBS}
	${GLFW_LIBRARIES}
)
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
set(OPENGL_LIBRARY
	${OPENGL_LIBRARY}
	${CMAKE_DL_LIBS}
	${GLFW_LIBRARIES}
)
endif(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

### GLEW ###

set(GLEW_SOURCE
	external_libraries/glew-1.13.0/src/glew.c
)

set(GLEW_HEADERS
)


add_library( GLEW_1130 STATIC
	${GLEW_SOURCE}
	${GLEW_INCLUDE}
)

target_link_libraries(GLEW_1130
	${OPENGL_LIBRARY}
	${EXTRA_LIBS}
)

### GLEW ###

set(GLAD_SOURCE
	external_libraries/glad/src/glad.c
)

set(GLAD_HEADERS
)


add_library( GLAD_LIB STATIC
	${GLAD_SOURCE}
	${GLAD_INCLUDE}
)

target_link_libraries(GLAD_LIB
	${OPENGL_LIBRARY}
	${EXTRA_LIBS}
)

set(OpenGL_GL_PREFERENCE GLVND)

add_definitions(
	-DTW_STATIC
	-DTW_NO_LIB_PRAGMA
	-DTW_NO_DIRECT3D
	-DGLEW_STATIC
	-D_CRT_SECURE_NO_WARNINGS
)

### stb_image ###
add_library(STB_IMAGE "external_libraries/stb_image/src/stb_image.c")
SET_TARGET_PROPERTIES(STB_IMAGE PROPERTIES LINKER_LANGUAGE C)


find_package(OpenGL REQUIRED)


add_executable(main src/main.cpp)
add_subdirectory(external_libraries/glfw)
add_subdirectory(external_libraries/glm)
include_directories( 
	${OPENGL_INCLUDE_DIRS}
	external_libraries/glew-1.13.0/include
	external_libraries/glad/include
	inc
	external_libraries/stb_image/include
	${CMAKE_BINARY_DIR}/config
)



target_include_directories(main
    PUBLIC external_libraries/glfw/include
	PUBLIC external_libraries/glew-1.13.0/include
	PUBLIC external_libraries/glad/include
	PUBLIC inc
	PUBLIC external_libraries/stb_image/include
	
)
target_link_directories(main
    PRIVATE external_libraries/glfw/src
	PRIVATE external_libraries/glew-1.13.0/src
	PRIVATE external_libraries/glad/src
	PRIVATE external_libraries/stb_image/src
)
target_link_libraries(main glfw ${OPENGL_LIBRARY}  GLEW_1130 GLAD_LIB STB_IMAGE)

