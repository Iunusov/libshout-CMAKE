cmake_minimum_required(VERSION 3.0)

project (shout)

option(WITH_SPEEX "WITH_SPEEX" OFF)
option(WITH_OPENSSL "WITH_OPENSSL" OFF)
option(WITH_THEORA "WITH_THEORA" OFF)
option(WITH_OGG "WITH_OGG" OFF)
option(WITH_VORBIS "WITH_VORBIS" OFF)

# Inspired from /usr/share/autoconf/autoconf/c.m4
FOREACH(KEYWORD "inline" "__inline__" "__inline")
   IF(NOT DEFINED C_INLINE)
     TRY_COMPILE(C_HAS_${KEYWORD} "${CMAKE_CURRENT_SOURCE_DIR}"
       "${CMAKE_CURRENT_SOURCE_DIR}/test_inline.c"
       COMPILE_DEFINITIONS "-Dinline=${KEYWORD}")
     IF(C_HAS_${KEYWORD})
       SET(C_INLINE TRUE)
       ADD_DEFINITIONS("-Dinline=${KEYWORD}")
     ENDIF(C_HAS_${KEYWORD})
   ENDIF(NOT DEFINED C_INLINE)
ENDFOREACH(KEYWORD)
IF(NOT DEFINED C_INLINE)
   ADD_DEFINITIONS("-Dinline=")
ENDIF(NOT DEFINED C_INLINE)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/common)

if(WIN32)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/win32compat)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/win32compat/shout)
endif()

if(WIN32)
add_definitions (-Dstrcasecmp=_stricmp)
endif()

if(NOT WIN32)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/include/shout/shout.h.in ${CMAKE_CURRENT_SOURCE_DIR}/include/shout/shout.h)
endif()

INCLUDE (CheckIncludeFiles)
INCLUDE (CheckFunctionExists)
INCLUDE (CheckIncludeFile)

CHECK_FUNCTION_EXISTS(endhostent HAVE_ENDHOSTENT)
if(HAVE_ENDHOSTENT) 
add_definitions(-DHAVE_ENDHOSTENT)
endif()

CHECK_FUNCTION_EXISTS(ftime HAVE_FTIME)
if(HAVE_FTIME) 
add_definitions(-DHAVE_FTIME)
endif()

CHECK_FUNCTION_EXISTS(getaddrinfo HAVE_GETADDRINFO)
if(HAVE_GETADDRINFO) 
add_definitions(-DHAVE_GETADDRINFO)
endif()

CHECK_FUNCTION_EXISTS(getnameinfo HAVE_GETNAMEINFO)
if(HAVE_GETNAMEINFO) 
add_definitions(-DHAVE_GETNAMEINFO)
endif()

CHECK_FUNCTION_EXISTS(gettimeofday HAVE_GETTIMEOFDAY)
if(HAVE_GETTIMEOFDAY) 
add_definitions(-DHAVE_GETTIMEOFDAY)
endif()


CHECK_FUNCTION_EXISTS(inet_aton HAVE_INET_ATON)
if(HAVE_INET_ATON) 
add_definitions(-DHAVE_INET_ATON)
endif()

CHECK_FUNCTION_EXISTS(inet_pton HAVE_INET_PTON)
if(HAVE_INET_PTON) 
add_definitions(-DHAVE_INET_PTON)
endif()

CHECK_INCLUDE_FILE(inttypes.h HAVE_INTTYPES_H)
if(HAVE_INTTYPES_H) 
add_definitions(-DHAVE_INTTYPES_H)
endif()

CHECK_INCLUDE_FILE(stdint.h HAVE_STDINT_H)
if(HAVE_STDINT_H) 
add_definitions(-DHAVE_STDINT_H)
endif()

CHECK_INCLUDE_FILE(memory.h HAVE_MEMORY_H)
if(HAVE_MEMORY_H) 
add_definitions(-DHAVE_MEMORY_H)
endif()

CHECK_FUNCTION_EXISTS(nanosleep HAVE_NANOSLEEP)
if(HAVE_NANOSLEEP) 
add_definitions(-DHAVE_NANOSLEEP)
endif()

CHECK_INCLUDE_FILE(pthread.h HAVE_PTHREAD)
if(HAVE_PTHREAD) 
add_definitions(-DHAVE_PTHREAD)
endif()

if(NOT HAVE_PTHREAD)
add_definitions (-DNO_THREAD)
endif()

CHECK_FUNCTION_EXISTS(pthread_spin_lock HAVE_PTHREAD_SPIN_LOCK)
if(HAVE_PTHREAD_SPIN_LOCK) 
add_definitions(-DHAVE_PTHREAD_SPIN_LOCK)
endif()

CHECK_FUNCTION_EXISTS(sethostent HAVE_SETHOSTENT)
if(HAVE_SETHOSTENT) 
add_definitions(-DHAVE_SETHOSTENT)
endif()

CHECK_INCLUDE_FILE(stdlib.h HAVE_STDLIB_H)
if(HAVE_STDLIB_H) 
add_definitions(-DHAVE_STDLIB_H)
endif()

CHECK_INCLUDE_FILE(strings.h HAVE_STRINGS_H)
if(HAVE_STRINGS_H) 
add_definitions(-DHAVE_STRINGS_H)
endif()

CHECK_INCLUDE_FILE(string.h HAVE_STRING_H)
if(HAVE_STRING_H) 
add_definitions(-DHAVE_STRING_H)
endif()

CHECK_INCLUDE_FILE(sys/select.h HAVE_SYS_SELECT_H)
if(HAVE_SYS_SELECT_H) 
add_definitions(-DHAVE_SYS_SELECT_H)
endif()

CHECK_INCLUDE_FILE(sys/socket.h HAVE_SYS_SOCKET_H)
if(HAVE_SYS_SOCKET_H) 
add_definitions(-DHAVE_SYS_SOCKET_H)
endif()

CHECK_INCLUDE_FILE(sys/stat.h HAVE_SYS_STAT_H)
if(HAVE_SYS_STAT_H) 
add_definitions(-DHAVE_SYS_STAT_H)
endif()

CHECK_INCLUDE_FILE(sys/timeb.h HAVE_SYS_TIMEB_H)
if(HAVE_SYS_TIMEB_H) 
add_definitions(-DHAVE_SYS_TIMEB_H)
endif()

CHECK_INCLUDE_FILE(sys/types.h HAVE_SYS_TYPES_H)
if(HAVE_SYS_TYPES_H) 
add_definitions(-DHAVE_SYS_TYPES_H)
endif()

CHECK_INCLUDE_FILE(sys/uio.h HAVE_SYS_UIO_H)
if(HAVE_SYS_UIO_H) 
add_definitions(-DHAVE_SYS_UIO_H)
endif()

CHECK_INCLUDE_FILE(unistd.h HAVE_UNISTD_H)
if(HAVE_UNISTD_H) 
add_definitions(-DHAVE_UNISTD_H)
endif()


CHECK_INCLUDE_FILE(winsock2.h HAVE_WINSOCK2_H)
if(HAVE_WINSOCK2_H) 
add_definitions(-DHAVE_WINSOCK2_H)
endif()

CHECK_FUNCTION_EXISTS(writev HAVE_WRITEV)
if(HAVE_WRITEV) 
add_definitions(-DHAVE_WRITEV)
endif()

set(PACKAGE_URL https://github.com/Iunusov/libshout-CMAKE)
set(PACKAGE_BUGREPORT https://github.com/Iunusov/libshout-CMAKE)
add_definitions (-DVERSION="2.4.1")
add_definitions (-DLIBSHOUT_MAJOR=2)
add_definitions (-DLIBSHOUT_MINOR=4)
add_definitions (-DLIBSHOUT_MICRO=1)
add_definitions (-DHAVE_GETTIMEOFDAY=1)
add_definitions (-DTIME_WITH_SYS_TIME)

if(HAVE_PTHREAD)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/thread SOURCE_LIB)
endif()
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/net SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/httpp SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/avl SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src/common/timing SOURCE_LIB)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src SOURCE_LIB)

if (NOT WITH_OPENSSL)
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/tls.c")
endif()

if(NOT WITH_SPEEX)
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/codec_speex.c")
endif()

if(NOT WITH_THEORA)
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/codec_theora.c")
endif()

if(NOT WITH_OGG)
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/codec_opus.c")
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/format_ogg.c")
endif()

if(NOT WITH_VORBIS)
list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/codec_vorbis.c")
endif()

list(REMOVE_ITEM SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/src/common/avl/test.c")

if(WIN32)
list(APPEND SOURCE_LIB "${CMAKE_CURRENT_SOURCE_DIR}/win32compat/gettimeofday.c")
endif()



add_library(${PROJECT_NAME} STATIC ${SOURCE_LIB})

target_include_directories(${PROJECT_NAME} 
PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
)

if(WIN32)
target_include_directories(${PROJECT_NAME} 
PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/win32compat/shout
)
endif()

if(WITH_VORBIS)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} vorbis)
endif()

if(WITH_OGG)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} ogg)
endif()

if(WITH_SPEEX)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} speex)
endif()

if(WITH_THEORA)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} theora)
endif()

if(WITH_OPENSSL)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} ssl)
endif()

if(WIN32)
  target_link_libraries(${PROJECT_NAME} wsock32 ws2_32)
endif()

set_property(TARGET ${PROJECT_NAME} PROPERTY POSITION_INDEPENDENT_CODE ON)

