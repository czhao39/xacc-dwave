cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
project(xacc-dwave LANGUAGES CXX)

option(DWAVE_BUILD_TESTS "Build test programs" OFF)

set(CMAKE_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 11)
message(STATUS "C++ version ${CXX_STANDARD} configured.")

find_package(XACC REQUIRED)

if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set (CMAKE_INSTALL_PREFIX "${XACC_ROOT}" CACHE PATH "default install path" FORCE )
endif()

set(CppMicroServicesConfig_DIR "${XACC_ROOT}/share/cppmicroservices4/cmake")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${XACC_ROOT}/share/xacc")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CppMicroServicesConfig_DIR}")
include(tests)
include(CppMicroServicesConfig)

include_directories(${XACC_INCLUDE_DIRS})
include_directories(${CMAKE_SOURCE_DIR}/utils)

link_directories(${XACC_LIBRARY_DIR})

if(${XACC_HAS_ANTLR})
   add_subdirectory(compiler)
endif()
add_subdirectory(accelerator)
add_subdirectory(tpls/xacc-cmr)
if(PYTHON_INCLUDE_DIR)
   add_subdirectory(python)
endif()

if(DWAVE_BUILD_TESTS)
   add_subdirectory(examples)
   include_directories(${CMAKE_SOURCE_DIR}/accelerator)
   include_directories(${CMAKE_SOURCE_DIR}/compiler)
   include_directories(${CMAKE_SOURCE_DIR}/compiler/runtime/src)
   add_subdirectory(tests)
endif()
   
