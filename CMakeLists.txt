# === CMAKE ====================================================================

# Version
cmake_minimum_required(VERSION 3.14.0)

# Includes
include(cmake/PreCMakeLists.cmake)

# Target
set(TARGET nuklear)
set(TARGET_LIST ${TARGET_LIST} ${TARGET})
project(${TARGET} VERSION 1.0.0)
add_library(${TARGET})

# === OPTION ===================================================================

target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_FIXED_TYPES          ON  "If defined it will include header <stdint.h> for fixed sized types otherwise nuklear tries to select the correct type. If that fails it will throw a compiler error and you have to select the correct types yourself.")
target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_DEFAULT_ALLOCATOR    ON  "If defined it will include header <stdlib.h> and provide additional functions to use this library without caring for memory allocation control and therefore ease memory management.")
target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_STANDARD_IO          ON  "If defined it will include header <stdio.h> and provide additional functions depending on file loading.")
target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_STANDARD_VARARGS	    ON  "If defined it will include header and provide additional functions depending on file loading.")
target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_VERTEX_BUFFER_OUTPUT ON  "Defining this adds a vertex draw command list backend to this library, which allows you to convert queue commands into vertex draw commands. This is mainly if you need a hardware accessible format for OpenGL, DirectX, Vulkan, Metal,...")
target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_FONT_BAKING          ON  "Defining this adds stb_truetype and stb_rect_pack implementation to this library and provides font baking and rendering. If you already have font handling or do not want to use this font handler you don't have to define it.")
target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_DEFAULT_FONT         ON  "Defining this adds the default font: ProggyClean.ttf into this library which can be loaded into a font atlas and allows using this library without having a truetype fontf")
target_add_bool_option(${TARGET} PUBLIC NK_INCLUDE_COMMAND_USERDATA     OFF "Defining this adds a userdata pointer into each command. Can be useful for example if you want to provide custom shaders depending on the used widget. Can be combined with the style structures.")
target_add_bool_option(${TARGET} PUBLIC NK_BUTTON_TRIGGER_ON_RELEASE    OFF "Different platforms require button clicks occurring either on buttons being pressed (up to down) or released (down to up). By default this library will react on buttons being pressed, but if you define this it will only trigger if a button is released.")
target_add_bool_option(${TARGET} PUBLIC NK_ZERO_COMMAND_MEMORY          OFF "Defining this will zero out memory for each drawing command added to a drawing queue (inside nk_command_buffer_push). Zeroing command memory is very useful for fast checking (using memcmp) if command buffers are equal and avoid drawing frames when nothing on screen has changed since previous frame.")
target_add_bool_option(${TARGET} PUBLIC NK_UINT_DRAW_INDEX              OFF "Defining this will set the size of vertex index elements when using NK_VERTEX_BUFFER_OUTPUT to 32bit instead of the default of 16bit")
target_add_bool_option(${TARGET} PUBLIC NK_KEYSTATE_BASED_INPUT         ON  "Define this if your backend uses key state for each frame rather than key press/release events")

# === TOOL =====================================================================

# C/C++ Standard

# Compile flags
target_compile_definitions(${TARGET}
    PRIVATE
    "NK_IMPLEMENTATION"
)

# === FILE =====================================================================

# Directories
set(${TARGET}_SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR})
set(${TARGET}_INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR})
set(${TARGET}_EXTERNAL_DIR ${CMAKE_CURRENT_LIST_DIR}/external)

# Sources
target_sources(${TARGET}
    PRIVATE
    ${${TARGET}_SOURCE_DIR}/nuklear.c
)

# Public headers
set(${TARGET}_PUBLIC_HEADERS
    ${${TARGET}_INCLUDE_DIR}/nuklear.h
)

# Include directories
target_include_directories(${TARGET}
    PUBLIC
    $<INSTALL_INTERFACE:include>

    PRIVATE
    ${${TARGET}_INCLUDE_DIR}
    ${${TARGET}_SOURCE_DIR}
    ${${TARGET}_EXTERNAL_DIR}
    ${CMAKE_BINARY_DIR}
)

# === DEPENDENCY ===============================================================

# Dependencies
# target_link_libraries(${TARGET}
#     PRIVATE
# )

# === TEST =====================================================================

# set(${TARGET}_TEST_SOURCES
# )

# set(${TARGET}_TEST_LINK_LIBRARIES
# )

# === CMAKE ====================================================================

include(cmake/PublicHeader.cmake)
include(cmake/ConfigHeader.cmake)
include(cmake/Install.cmake)
include(cmake/PackageConfig.cmake)
