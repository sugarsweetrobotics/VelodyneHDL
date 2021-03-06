cmake_minimum_required(VERSION 2.8.7)

# VelodyneHDL Driver
project(VelodyneHDL)

find_package(Boost REQUIRED COMPONENTS system thread)

if(WIN32)
set(WINPCAPS_ROOT "${PROJECT_SOURCE_DIR}/../winpcap")
include_directories(${WINPCAPS_ROOT}/include)
link_directories(${WINPCAPS_ROOT}/lib/x64)
set(PCAP_LIB wpcap)
else(WIN32)
set(PCAP_LIB pcap)
endif(WIN32)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${Boost_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIR})
include_directories(${Boost_INCLUDE_DIR}/..)
link_directories(${Boost_LIBRARY_DIRS})

add_library(PacketDriver SHARED PacketDriver.cpp)
target_link_libraries(PacketDriver
  ${Boost_SYSTEM_LIBRARY}
)

add_library(PacketDecoder SHARED PacketDecoder.cpp)
target_link_libraries(PacketDecoder
)

add_library(PacketBundler SHARED PacketBundler.cpp)
target_link_libraries(PacketBundler
)

add_library(PacketBundleDecoder SHARED PacketBundleDecoder.cpp)
target_link_libraries(PacketBundleDecoder
)

add_executable(test_PacketDriver tests/test_PacketDriver.cpp)
target_link_libraries(test_PacketDriver
  PacketDriver
  ${Boost_SYSTEM_LIBRARY}
)

add_executable(test_PacketDecoder tests/test_PacketDecoder.cpp)
target_link_libraries(test_PacketDecoder
  PacketDriver
  PacketDecoder
  ${PCAP_LIB}
)

add_executable(test_PacketWriter tests/test_PacketWriter.cpp)
target_link_libraries(test_PacketWriter
  PacketDriver
  ${PCAP_LIB}
)

add_executable(test_PacketBundler tests/test_PacketBundler.cpp)
target_link_libraries(test_PacketBundler
  PacketDriver
  PacketBundler
)

add_executable(test_PacketBundleDecoder tests/test_PacketBundleDecoder.cpp)
target_link_libraries(test_PacketBundleDecoder
  PacketDriver
  PacketBundler
  PacketBundleDecoder
)

add_executable(PacketFileSender PacketFileSender.cxx)
target_link_libraries(PacketFileSender
  ${Boost_SYSTEM_LIBRARY}
  ${Boost_THREAD_LIBRARY}
  ${PCAP_LIB}
)
