cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

set(CURRENT_PROJECT_VERSION 0.9.0)
#add_definitions(-DPROJECT_VERSION="${CURRENT_PROJECT_VERSION}")
#if (POLICY "CMP0048")
#    cmake_policy(SET "CMP0048" "NEW")
#endif (POLICY "CMP0048")
project(mbus C)

option(BUILD_SHARED_LIBS "Build shared libraries" ON)
#if (${BUILD_SHARED_LIBS})
#    set(DEFS -DDLLBUILD)
#endif(${BUILD_SHARED_LIBS})

set(LIB_HEADERS
    ${PROJECT_SOURCE_DIR}/mbus/mbus-protocol-aux.h
    ${PROJECT_SOURCE_DIR}/mbus/mbus-protocol.h
    ${PROJECT_SOURCE_DIR}/mbus/mbus-serial.h
    ${PROJECT_SOURCE_DIR}/mbus/mbus-tcp.h
    ${PROJECT_SOURCE_DIR}/mbus/mbus.h)
#file(GLOB LIB_SRCS "${PROJECT_SOURCE_DIR}/src/*.c")

#set(CMAKE_INCLUDE_CURRENT_DIR OFF)

#if("${CMAKE_SYSTEM_NAME}" STREQUAL "Windows")
#    enable_language(RC)
#    #set(INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/src/win32)
#    set(DEFS ${DEFS} -D_CRT_SECURE_NO_WARNINGS)
#    set(LINK_LIBS "Ws2_32")
#    set(RCFILE "${PROJECT_SOURCE_DIR}/src/win32/modbus.rc")
#else("${CMAKE_SYSTEM_NAME}" STREQUAL "Windows")
    #set(INCLUDE_DIRS ${PROJECT_SOURCE_DIR})
    #set(DEFS ${DEFS})
#endif("${CMAKE_SYSTEM_NAME}" STREQUAL "Windows")

#add_definitions(${DEFS})

add_library(mbus
    mbus/mbus-protocol-aux.c
    mbus/mbus-protocol.c
    mbus/mbus-serial.c
    mbus/mbus-tcp.c
    mbus/mbus.c
    )


target_include_directories(mbus
    PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    )

install(TARGETS mbus EXPORT mbusConfig
    ARCHIVE  DESTINATION lib
    LIBRARY  DESTINATION lib
    RUNTIME  DESTINATION bin)  # This is for Windows
install(FILES ${LIB_HEADERS} DESTINATION include)

# This makes the project importable from the install directory
# Put config file in per-project dir (name MUST match), can also
# just go into 'cmake'.
install(EXPORT mbusConfig DESTINATION share/mbus/cmake)

# This makes the project importable from the build directory
export(TARGETS mbus FILE mbusConfig.cmake)

# This registers the package with cmake user registry
export(PACKAGE mbus)
