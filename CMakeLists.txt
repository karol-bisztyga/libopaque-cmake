PROJECT(libopaque-cmake C CXX)

include(FetchContent)

cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wall --std=c++11 -O3 -fPIC")
set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -Wall -O3 -fPIC")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY bin)
set(PACKAGE_NAME "libopaque-cmake")
set(CMAKE_CXX_STANDARD 14)

FetchContent_Declare(Sodium
  GIT_REPOSITORY https://github.com/robinlinden/libsodium-cmake.git
)
set(SODIUM_DISABLE_TESTS ON)
FetchContent_MakeAvailable(Sodium)

if(NOT DEFINED LIBOPAQUE_PATH_PREFIX)
  set(LIBOPAQUE_PATH_PREFIX .)
endif()

include_directories(
  ${LIBOPAQUE_PATH_PREFIX}/src
  ${LIBOPAQUE_PATH_PREFIX}/src/aux/
)

add_library(
  ${PACKAGE_NAME}
  SHARED
  ${LIBOPAQUE_PATH_PREFIX}/src/aux/kdf_hkdf_sha256.c
  ${LIBOPAQUE_PATH_PREFIX}/src/common.c
  ${LIBOPAQUE_PATH_PREFIX}/src/opaque.c
)

target_link_libraries(
  ${PACKAGE_NAME}
  sodium
)
