cmake_minimum_required(VERSION 3.8)

project(ygo)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/macros")

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build." FORCE)
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()


if (MSVC)
    include_directories ( ./lua )
else ()
    include_directories ( ./ocgcore/lua/src )
endif ()

file(GLOB_RECURSE AUTO_FILES_RESULT
    "ocgcore/*.cpp"
    "ocgcore/*.c"
    "ocgcore/*.h"
)
list(REMOVE_ITEM AUTO_FILES_RESULT
    "${CMAKE_SOURCE_DIR}/ocgcore/lua/src/lua.c"
    "${CMAKE_SOURCE_DIR}/ocgcore/lua/src/luac.c"
)
add_library (ocgcore SHARED ${AUTO_FILES_RESULT})
include(GenerateExportHeader)
generate_export_header(ocgcore
    BASE_NAME ocgcore
    EXPORT_MACRO_NAME OCGCORE_API
    EXPORT_FILE_NAME ocgcore_export.h
)

target_include_directories(ocgcore PUBLIC
    ${CMAKE_CURRENT_BINARY_DIR}
)


