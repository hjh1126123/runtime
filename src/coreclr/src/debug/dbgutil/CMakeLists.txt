set(CMAKE_INCLUDE_CURRENT_DIR ON)

if(CLR_CMAKE_HOST_WIN32)
  #use static crt
  set(CMAKE_MSVC_RUNTIME_LIBRARY MultiThreaded)
endif(CLR_CMAKE_HOST_WIN32)

if(CLR_CMAKE_HOST_WIN32 OR CLR_CMAKE_HOST_OSX)
  include_directories(${CLR_DIR}/src/inc/llvm)
endif(CLR_CMAKE_HOST_WIN32 OR CLR_CMAKE_HOST_OSX)

add_definitions(-DPAL_STDCPP_COMPAT)

if(CLR_CMAKE_TARGET_ALPINE_LINUX)
    add_definitions(-DTARGET_ALPINE_LINUX)
endif(CLR_CMAKE_TARGET_ALPINE_LINUX)

set(DBGUTIL_SOURCES
    dbgutil.cpp
)

if(CLR_CMAKE_TARGET_LINUX)
    list(APPEND DBGUTIL_SOURCES
        elfreader.cpp
    )
endif(CLR_CMAKE_TARGET_LINUX)

if(CLR_CMAKE_HOST_OSX)
    list(APPEND DBGUTIL_SOURCES
        machoreader.cpp
    )
endif(CLR_CMAKE_HOST_OSX)

add_library_clr(dbgutil STATIC ${DBGUTIL_SOURCES})
