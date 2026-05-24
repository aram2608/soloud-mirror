cmake_minimum_required(VERSION 3.21)
project(soloud LANGUAGES C CXX)

option(SOLOUD_BACKEND_SDL2_STATIC "Use SDL2 static backend" ON)

file(GLOB SOLOUD_SOURCES
    src/core/*.cpp
    src/audiosource/wav/*.cpp
)
# stb_vorbis is a .c file — must be listed explicitly, globs on *.cpp miss it
list(APPEND SOLOUD_SOURCES src/audiosource/wav/stb_vorbis.c)

if(SOLOUD_BACKEND_SDL2_STATIC)
    file(GLOB BACKEND_SOURCES src/backend/sdl2_static/*.cpp)
    list(APPEND SOLOUD_SOURCES ${BACKEND_SOURCES})
endif()

add_library(soloud STATIC ${SOLOUD_SOURCES})

target_include_directories(soloud PUBLIC include)

if(SOLOUD_BACKEND_SDL2_STATIC)
    find_package(SDL2 REQUIRED)
    target_compile_definitions(soloud PUBLIC WITH_SDL2_STATIC)
    target_link_libraries(soloud PUBLIC SDL2::SDL2)
endif()
