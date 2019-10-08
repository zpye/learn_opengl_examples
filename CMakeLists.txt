cmake_minimum_required(VERSION 3.0)

project(opengl-tutorial)

# opengl libs
add_subdirectory(glfw-3.3)

add_subdirectory(glm)
set_target_properties(glm_static PROPERTIES FOLDER "GLM")

set(glad_dir ${CMAKE_SOURCE_DIR}/glad)
add_library(glad
${glad_dir}/glad.c
${glad_dir}/glad.h
${glad_dir}/khrplatform.h
)
target_include_directories(glad PUBLIC ${CMAKE_SOURCE_DIR})
set_target_properties(glad PROPERTIES FOLDER "GLAD")

set(GL_LIBS glm_static glad glfw)

option(BUILD_ASSIMP "build Open Asset Import Library" OFF)
if(BUILD_ASSIMP)
    add_subdirectory(assimp)
    set_target_properties(assimp PROPERTIES FOLDER "ASSIMP")
    set_target_properties(IrrXML PROPERTIES FOLDER "ASSIMP")
    set_target_properties(zlib PROPERTIES FOLDER "ASSIMP")
    set_target_properties(zlibstatic PROPERTIES FOLDER "ASSIMP")
    list(APPEND GL_LIBS assimp)
endif(BUILD_ASSIMP)

# options
add_compile_options(-DSHADER_PATH="${CMAKE_SOURCE_DIR}/resources")
add_compile_options(-DGLFW_INCLUDE_NONE)

include_directories(${CMAKE_SOURCE_DIR})

# executions
add_executable(1_hello_window 1_hello_window.cpp)
target_link_libraries(1_hello_window ${GL_LIBS})

add_executable(2_triangle_indexed 2_triangle_indexed.cpp)
target_link_libraries(2_triangle_indexed ${GL_LIBS})

add_executable(2_two_triangle 2_two_triangle.cpp)
target_link_libraries(2_two_triangle ${GL_LIBS})

add_executable(3_shader 3_shader_class.cpp shader_s.h)
target_link_libraries(3_shader ${GL_LIBS})

add_executable(4_texture 4_textures_combined.cpp shader_s.h stb_image.h)
target_link_libraries(4_texture ${GL_LIBS})

add_executable(5_transformations 5_transformations.cpp shader_s.h stb_image.h)
target_link_libraries(5_transformations ${GL_LIBS})

add_executable(6_coordinate_systems_multiple 6_coordinate_systems_multiple.cpp shader_m.h stb_image.h)
target_link_libraries(6_coordinate_systems_multiple ${GL_LIBS})

add_executable(7_camera_class 7_camera_class.cpp camera.h shader_m.h stb_image.h)
target_link_libraries(7_camera_class ${GL_LIBS})

add_executable(8_basic_lighting_specular 8_basic_lighting_specular.cpp camera.h shader_m.h)
target_link_libraries(8_basic_lighting_specular ${GL_LIBS})

add_executable(9_materials 9_materials.cpp camera.h shader_m.h)
target_link_libraries(9_materials ${GL_LIBS})

add_executable(10_multiple_lights 10_multiple_lights.cpp camera.h shader_m.h stb_image.h)
target_link_libraries(10_multiple_lights ${GL_LIBS})

if(BUILD_ASSIMP)
    add_executable(11_model_loading 11_model_loading.cpp camera.h shader.h model.h mesh.h stb_image.h)
    target_link_libraries(11_model_loading ${GL_LIBS})
endif(if(BUILD_ASSIMP))