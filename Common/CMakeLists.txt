cmake_minimum_required(VERSION 2.8)

project(Common)

find_package(ImageMagick COMPONENTS Magick++)

if( MINGW )
    set(glut freeglut)
    set(GL openGL32)
    set(GLEW GLEW32)

    include_directories(../Include ${ImageMagick_INCLUDE_DIRS})

    link_libraries(glut glfw assimp AntTweakBar)
else()
    set(glut glut)
    set(GL GL)
    set(GLEW GLEW)

    find_package(PkgConfig REQUIRED)

    pkg_search_module(GLFW REQUIRED glfw3)

    include_directories(../Include ${ImageMagick_INCLUDE_DIRS} ${GLFW_INCLUDE_DIRS})

    link_libraries(glut ${GLFW_LIBRARIES} assimp AntTweakBar)
endif()

add_library(math_3d SHARED math_3d.cpp)
add_library(pipeline SHARED pipeline.cpp)
add_library(backend SHARED ogldev_backend.cpp glut_backend.cpp ogldev_glfw_backend.cpp)
add_library(app SHARED ogldev_app.cpp)
add_library(io_buffer SHARED io_buffer.cpp)
add_library(util SHARED ogldev_util.cpp)
add_library(basic_lighting SHARED ogldev_basic_lighting.cpp)
add_library(shadow_map_fbo SHARED ogldev_shadow_map_fbo.cpp)
add_library(atb SHARED ogldev_atb.cpp)

add_library(texture SHARED ogldev_texture.cpp random_texture.cpp cubemap_texture.cpp)
target_link_libraries(texture atb ${ImageMagick_LIBRARIES})

add_library(camera SHARED camera.cpp)
target_link_libraries(camera atb)

add_library(basic_mesh SHARED ogldev_basic_mesh.cpp)
target_link_libraries(basic_mesh texture)

add_library(skinned_mesh SHARED ogldev_skinned_mesh.cpp)
target_link_libraries(skinned_mesh texture)

add_library(technique SHARED technique.cpp)
target_link_libraries(technique util ${GLEW})
