cmake_minimum_required(VERSION 3.0.0)

project(Olympus)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(vulkan)
include(glfw)
include(imgui)
include(buildshaders)

add_library(Olympus OBJECT)

target_compile_features(Olympus PRIVATE cxx_std_17)

# ###########################
# Olympus - Compiler flags #
# ###########################
include(CompilerFlags)
add_compiler_flags(Olympus PRIVATE)

# ############################
# Olympus - Finding Vulkan #
# ############################
set(
    OLYMPUS_LINKER_FLAGS

    ${OLYMPUS_LINKER_FLAGS}
    ${Vulkan_LIBRARY}
)

target_include_directories(
    Olympus
    SYSTEM
    PUBLIC

    ${Vulkan_INCLUDE_DIRS}
)

# #########################
# OLYMPUS - Source files #
# #########################
set(
    OLYMPUS_FILES

    sources/*.cpp
    include/*.h
    include/*.inl
)

# Adding every file to be compiled
file(
    GLOB_RECURSE
    OLYMPUS_SOURCES

    ${OLYMPUS_FILES}
)

# ###################
# Olympus - Build #
# ###################
target_sources(Olympus PRIVATE ${OLYMPUS_SOURCES})

target_include_directories(
    Olympus

    SYSTEM
    PUBLIC

    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

set(
    OLYMPUS_LINKER_FLAGS

    ${OLYMPUS_LINKER_FLAGS}
    glfw::glfw
    imgui::imgui
)

target_compile_definitions(
    Olympus

    PRIVATE

    OLYMPUS_SHADERS="${CMAKE_CURRENT_SOURCE_DIR}/shaders/build"
)

target_compile_options(Olympus PUBLIC ${OLYMPUS_COMPILER_FLAGS})
target_link_libraries(Olympus PUBLIC ${OLYMPUS_LINKER_FLAGS})
