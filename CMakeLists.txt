cmake_minimum_required(VERSION 3.0)

project (libshout)

add_subdirectory(OGG-Vorbis-CMAKE)

if(WIN32)
add_definitions (-DNO_THREAD)
add_definitions (-DHAVE_WINSOCK2_H)
add_definitions (-Dstrcasecmp=_stricmp)
endif()
add_definitions (-DVERSION="2.4.1")
add_definitions (-DLIBSHOUT_MAJOR=2)
add_definitions (-DLIBSHOUT_MINOR=4)
add_definitions (-DLIBSHOUT_MICRO=1)
add_definitions (-DHAVE_GETTIMEOFDAY=1)
add_definitions (-DHAVE_INTTYPES_H=1)

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/net SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/httpp SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/avl SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/timing SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src SOURCE_LIB)

list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/tls.c")
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/codec_speex.c")
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/codec_theora.c")
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/common/avl/test.c")

if(WIN32)
list(APPEND SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/win32compat/gettimeofday.c")
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/common)

if(WIN32)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/win32compat)
endif()

add_library(${PROJECT_NAME} STATIC ${SOURCE_LIB})

target_include_directories(${PROJECT_NAME} 
PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
)

TARGET_LINK_LIBRARIES(${PROJECT_NAME} libvorbis)

if(WIN32)
  target_link_libraries(${PROJECT_NAME} wsock32 ws2_32)
endif()

set_property(TARGET ${PROJECT_NAME} PROPERTY POSITION_INDEPENDENT_CODE ON)

