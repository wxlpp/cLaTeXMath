cmake_minimum_required(VERSION 3.5)
project(iOSLaTeXMath)


# check if compiler has c++11 support

include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no c++11 support. Please use a different one that supports c++11.")
endif()

# copy res dir

file(COPY res DESTINATION .)

# source files
include_directories("src")

file(GLOB_RECURSE SOURCES "src/*.cpp")

file(GLOB_RECURSE HEADERS "src/*.h")


add_library (${PROJECT_NAME} SHARED ${SOURCES} ${HEADERS})


set_xcode_property(${PROJECT_NAME} GCC_GENERATE_DEBUGGING_SYMBOLS YES "All")

# compile options

option(HAVE_LOG "If enable log" ON)
if(HAVE_LOG)
    add_definitions(-DHAVE_LOG)
endif()

option(GRAPHICS_DEBUG "If enable graphics debug" ON)
if(GRAPHICS_DEBUG)
    add_definitions(-DGRAPHICS_DEBUG)
endif()

option(MEM_CHECK "If compile for memory check only" OFF)
if(MEM_CHECK)
    add_definitions(-DMEM_CHECK)
endif()

set_target_properties(${PROJECT_NAME} PROPERTIES
FRAMEWORK TRUE
FRAMEWORK_VERSION A
MACOSX_FRAMEWORK_IDENTIFIER cn.tvm.tvm_model
# MACOSX_FRAMEWORK_INFO_PLIST Info.plist
# "current version" in semantic format in Mach-O binary file
VERSION 1.0.0
# "compatibility version" in semantic format in Mach-O binary file
SOVERSION 1.0.0
XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY "iPhone Developer"
RESOURCE "res"
)
# Installation
# install (TARGETS iOSLaTeXMath DESTINATION lib)
# install (FILES ${HEADERS} DESTINATION include)
