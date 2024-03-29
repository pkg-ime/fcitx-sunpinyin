cmake_minimum_required (VERSION 2.6)

project(fcitx-sunpinyin)

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

option(ENABLE_TEST "enable function test" Off)

FIND_PACKAGE(Libintl REQUIRED)
FIND_PACKAGE(Gettext REQUIRED)
FIND_PACKAGE(Fcitx 4.2.0 REQUIRED)

PKG_CHECK_MODULES(SUNPINYIN REQUIRED "sunpinyin-2.0 >= 2.0.4" )

# uninstall target
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)

set(CMAKE_C_FLAGS "-Wall -Wextra -Wno-sign-compare -Wno-unused-parameter -fvisibility=hidden ${CMAKE_C_FLAGS}")
set(CMAKE_CXX_FLAGS "-Wall -Wextra -Wno-sign-compare -Wno-unused-parameter -fvisibility=hidden ${CMAKE_CXX_FLAGS}")
set(CMAKE_SHARED_LINKER_FLAGS "-Wl,--as-needed ${CMAKE_SHARED_LINKER_FLAGS}")
set(CMAKE_MODULE_LINKER_FLAGS "-Wl,--as-needed ${CMAKE_MODULE_LINKER_FLAGS}")

if(NOT DEFINED LIB_INSTALL_DIR)
    set(LIB_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/lib)
endif()

set(HAVE_CONFIG_H)

set(libdir ${LIB_INSTALL_DIR})

add_subdirectory(src)
add_subdirectory(data)
add_subdirectory(po)
if (ENABLE_TEST)
    enable_testing()
    add_subdirectory(test)
endif (ENABLE_TEST)
