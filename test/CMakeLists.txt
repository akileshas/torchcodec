cmake_minimum_required(VERSION 3.14)
include(CMakePrintHelpers)
project(TorchCodecTests)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED)

find_package(Torch REQUIRED)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")


include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.14.0.zip
)
FetchContent_MakeAvailable(googletest)

add_executable(
  VideoDecoderTest
  VideoDecoderTest.cpp
)

target_include_directories(VideoDecoderTest SYSTEM PRIVATE ${TORCH_INCLUDE_DIRS})
target_include_directories(VideoDecoderTest SYSTEM PRIVATE ${libav_include_dirs})
target_include_directories(VideoDecoderTest PRIVATE ../)

target_link_libraries(
  VideoDecoderTest
  ${libtorchcodec_library_name}
  ${libtorchcodec_custom_ops_name}
  GTest::gtest_main
)

include(GoogleTest)
gtest_discover_tests(VideoDecoderTest)
