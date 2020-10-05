cmake_minimum_required(VERSION 3.10 FATAL_ERROR)
project("Mapbox GL Native Node Bindings" LANGUAGES CXX)

cmake_policy(SET CMP0063 NEW)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_VISIBILITY_INLINES_HIDDEN 1)

if(NOT EXISTS ${PROJECT_SOURCE_DIR}/node_modules/@mapbox/cmake-node-module/module.cmake)
    message("-- Not building node bindings, dependencies not found. Run 'npm install'.")
    message("-- ${PROJECT_SOURCE_DIR}/node_modules/@mapbox/cmake-node-module/module.cmake")
    return()
endif()

add_subdirectory(${PROJECT_SOURCE_DIR}/mapbox-gl-native EXCLUDE_FROM_ALL)

set(NODE_MODULE_CACHE_DIR ${CMAKE_SOURCE_DIR}/build/headers)
include(${PROJECT_SOURCE_DIR}/node_modules/@mapbox/cmake-node-module/module.cmake)

add_node_module(
    mbgl-node
    INSTALL_PATH ${PROJECT_SOURCE_DIR}/lib/{node_abi}/mbgl.node
    NAN_VERSION 2.14.1
    EXCLUDE_NODE_ABIS
        46
        47
        48
        51
        59
        67
        72
        79
        83
)

target_sources(
    mbgl-node
    INTERFACE
        ${PROJECT_SOURCE_DIR}/src/node_conversion.hpp
        ${PROJECT_SOURCE_DIR}/src/node_expression.cpp
        ${PROJECT_SOURCE_DIR}/src/node_expression.hpp
        ${PROJECT_SOURCE_DIR}/src/node_feature.cpp
        ${PROJECT_SOURCE_DIR}/src/node_feature.hpp
        ${PROJECT_SOURCE_DIR}/src/node_logging.cpp
        ${PROJECT_SOURCE_DIR}/src/node_logging.hpp
        ${PROJECT_SOURCE_DIR}/src/node_map.cpp
        ${PROJECT_SOURCE_DIR}/src/node_map.hpp
        ${PROJECT_SOURCE_DIR}/src/node_mapbox_gl_native.cpp
        ${PROJECT_SOURCE_DIR}/src/node_request.cpp
        ${PROJECT_SOURCE_DIR}/src/node_request.hpp
        ${PROJECT_SOURCE_DIR}/src/util/async_queue.hpp
)

target_link_libraries(
    mbgl-node
    INTERFACE Mapbox::Map
)
