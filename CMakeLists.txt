PROJECT(LibOpaque C CXX)

include(FetchContent)

cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wall --std=c++11 -O3 -fPIC")
set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -Wall -O3 -fPIC")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY bin)
set(PACKAGE_NAME "lib_opaque")
set(CMAKE_CXX_STANDARD 14)

FetchContent_Declare(Sodium
  GIT_REPOSITORY https://github.com/robinlinden/libsodium-cmake.git
)
set(SODIUM_DISABLE_TESTS ON)
FetchContent_MakeAvailable(Sodium)

include_directories(
  ./src
  ./src/aux/
)

add_library(
  ${PACKAGE_NAME}
  SHARED
  ./src/aux/kdf_hkdf_sha256.c
  ./src/common.c
  ./src/opaque.c
)

target_link_libraries(
  ${PACKAGE_NAME}
  sodium
)
