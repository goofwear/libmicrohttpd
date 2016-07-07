cmake_minimum_required(VERSION 3.2)

project(libmicrohttpd LANGUAGES C)

set(libmicrohttpd_VERSION 0.1)

if(MSVC)
  set(CMAKE_DEBUG_POSTFIX "d")
endif()


add_library(libmicrohttpd STATIC
  src/microhttpd/md5.c
  src/microhttpd/md5.h
  src/microhttpd/memorypool.c
  src/microhttpd/memorypool.h
  src/microhttpd/mhd_byteorder.h
  src/microhttpd/mhd_limits.h
  src/microhttpd/mhd_mono_clock.c
  src/microhttpd/mhd_mono_clock.h
  src/microhttpd/mhd_str.c
  src/microhttpd/mhd_str.h
  src/microhttpd/postprocessor.c
  src/microhttpd/reason_phrase.c
  src/microhttpd/response.c
  src/microhttpd/response.h
  src/microhttpd/sysfdsetsize.c
  src/microhttpd/sysfdsetsize.h
  src/microhttpd/tsearch.c
  src/microhttpd/tsearch.h
  src/microhttpd/base64.c
  src/microhttpd/base64.h
  src/microhttpd/basicauth.c
  src/microhttpd/connection.c
  src/microhttpd/connection.h
  src/microhttpd/daemon.c
  src/microhttpd/digestauth.c
  src/microhttpd/internal.c
  src/microhttpd/internal.h
  src/platform/w32functions.c
  src/include/w32functions.h
  src/include/autoinit_funcs.h
  src/include/microhttpd.h
  src/include/platform.h
  src/include/platform_interface.h
  w32/common/MHD_config.h 
)
target_include_directories(
  libmicrohttpd PRIVATE
  $<BUILD_INTERFACE:src/include;src/microhttpd;src/platform;w32/common>
  INTERFACE
  $<INSTALL_INTERFACE:include>
)

if(MSVC)
  target_compile_definitions(libmicrohttpd PRIVATE MHD_W32LIB)
endif()

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/libmicrohttpd-config-version.cmake
  VERSION ${libmicrohttpd_VERSION}
  COMPATIBILITY AnyNewerVersion
)

install(TARGETS libmicrohttpd EXPORT libmicrohttpd
  RUNTIME DESTINATION bin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)


install(FILES
  src/include/microhttpd.h
  DESTINATION include)

install(EXPORT libmicrohttpd
  FILE
    libmicrohttpd.cmake
  NAMESPACE
    libmicrohttpd::
  DESTINATION
    lib/cmake/libmicrohttpd
)
install(
  FILES
    cmake/libmicrohttpd-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/libmicrohttpd-config-version.cmake
  DESTINATION
    lib/cmake/libmicrohttpd
)