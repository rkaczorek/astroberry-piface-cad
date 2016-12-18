cmake_minimum_required(VERSION 2.4.7)

if(COMMAND cmake_policy)
    cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

PROJECT(indi-piface CXX C)
include(ExternalProject)

set (VERSION_MAJOR 2)
set (VERSION_MINOR 0)

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/")
set(BIN_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/bin")
set(CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS}")
set(MAKE make)

find_package(INDI REQUIRED)

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${INDI_INCLUDE_DIR})

include_directories(${CMAKE_SOURCE_DIR}/libs/libmcp23s17/src)
include_directories(${CMAKE_SOURCE_DIR}/libs/libpifacecad/src)

add_library(mcp23s17 STATIC IMPORTED)
set_property(TARGET mcp23s17 PROPERTY IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/libmcp23s17/libmcp23s17.a)
add_dependencies(mcp23s17 libmcp23s17)

add_library(pifacecad STATIC IMPORTED)
set_property(TARGET pifacecad PROPERTY IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/libpifacecad/libpifacecad.a)
add_dependencies(pifacecad libpifacecad)


################ libmcp23s17 ################

ExternalProject_Add(libmcp23s17
#    GIT_REPOSITORY "https://github.com/piface/libmcp23s17.git"
#    GIT_TAG "master"
    SOURCE_DIR ${CMAKE_SOURCE_DIR}/libs/libmcp23s17
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/libmcp23s17
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ${MAKE}
    INSTALL_COMMAND ""
    BUILD_IN_SOURCE 1
)


################ libpifacecad ################

ExternalProject_Add(libpifacecad
#    GIT_REPOSITORY "https://github.com/piface/libpifacecad.git"
#    GIT_TAG "master"
    SOURCE_DIR ${CMAKE_SOURCE_DIR}/libs/libpifacecad
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/libpifacecad
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ${MAKE}
    INSTALL_COMMAND ""
    BUILD_IN_SOURCE 1
)

################ PiFace CAD ################

set(indi_piface_cad_SRCS
        ${CMAKE_CURRENT_SOURCE_DIR}/piface_cad.cpp
   )

add_executable(indi_piface_cad ${indi_piface_cad_SRCS})
target_link_libraries(indi_piface_cad indidriver pifacecad mcp23s17)
install(TARGETS indi_piface_cad RUNTIME DESTINATION bin )
install(FILES indi_piface_cad.xml DESTINATION ${INDI_DATA_DIR})

