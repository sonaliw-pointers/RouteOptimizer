################################################################################
# project setting
################################################################################
project (RouteOptimizer)
cmake_minimum_required(VERSION 3.11)

project(ModernCMakeExample VERSION 1.0 LANGUAGES CXX)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

include_directories(${CMAKE_SOURCE_DIR}/src)
include_directories(${CMAKE_SOURCE_DIR}/include)

################################################################################
# CMake build options
################################################################################

set(BUILD_TEST        true  CACHE BOOL "build test")
set(BUILD_DOC         false CACHE BOOL "build document")


################################################################################
# include CMake module
################################################################################
include(libutils)
include(CMakeDependentOption)
include(CMakeForceCompiler)
include(FindDoxygen)

################################################################################
# set common compile flags
################################################################################
set(CMAKE_CXX_STANDARD 11)



################################################################################
# add subdirectories
################################################################################
add_subdirectory(src)
if(${BUILD_TEST})
  enable_testing()
  add_subdirectory(test)
endif(${BUILD_TEST})

################################################################################
# documentation
################################################################################
if(${BUILD_DOC})

  configure_file(
    ${CMAKE_SOURCE_DIR}/docs/Doxyfile.in
    ${CMAKE_BINARY_DIR}/Doxyfile
    NEWLINE_STYLE LF)

  configure_file(
    ${CMAKE_SOURCE_DIR}/docs/Doxyfile-internal.in
    ${CMAKE_BINARY_DIR}/Doxyfile-internal
    NEWLINE_STYLE LF)

  find_package(Doxygen)
  if(DOXYGEN_FOUND)
    add_custom_target(docs ALL
      COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_BINARY_DIR}/Doxyfile
      COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_BINARY_DIR}/Doxyfile-internal
      WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
  endif(DOXYGEN_FOUND)

  install(DIRECTORY ${CMAKE_SOURCE_DIR}/EULA.txt DESTINATION docs)
  install(DIRECTORY ${CMAKE_SOURCE_DIR}/COPYRIGHT.txt DESTINATION docs)
  install(DIRECTORY ${CMAKE_BINARY_DIR}/html DESTINATION docs)
endif(${BUILD_DOC})
# TODO : any other documents ...
# TODO : add COPYRIGHT

################################################################################
# public headers
################################################################################
install(DIRECTORY include/ DESTINATION include)