project(cryptoapi)
cmake_minimum_required(VERSION 3.5)
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
set(CMAKE_CXX_STANDARD 11)

set(VERSION_MAJOR "0")
set(VERSION_MINOR "1")
set(VERSION_PATCH "0")

set(VERSION_STRING "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")

find_package(LIBWEBSOCKETS REQUIRED)
find_package(CURL REQUIRED)
find_package(Boost COMPONENTS system filesystem program_options iostreams unit_test_framework REQUIRED)
find_package(Threads REQUIRED)
find_package(TBB REQUIRED)
find_package(ZLIB REQUIRED)

set(LINK_LIBS ${Boost_LIBRARIES} ${LIBWEBSOCKETS_LIBRARIES}
  ${CURL_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT} ${TBB_LIBRARIES} ${ZLIB_LIBRARIES} -lrt -lssl -lcrypto)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -p -Wall -Wno-unused-variable -Wno-braced-scalar-init -O3 ${RAPIDJSON_CXX_FLAGS} -march=native")

include_directories(${PROJECT_SOURCE_DIR}/src/)
include_directories(${PROJECT_SOURCE_DIR}/src/bitstamp/)
include_directories(${PROJECT_SOURCE_DIR}/src/kraken/)
include_directories(${PROJECT_SOURCE_DIR}/pjson/)
include_directories(${TBB_INCLUDE_DIRS} ${Boost_INCLUDE_DIR} ${LIBWEBSOCKETS_INCLUDE_DIR}
                    ${CURL_INCLUDE_DIR} ${RAPIDJSON_INCLUDE_DIRS})
link_directories(${LIBWEBSOCKETS_LIBRARY_DIR} ${CURL_LIBRARY_DIR})

file(GLOB_RECURSE BITSTAMP_SOURCES ${PROJECT_SOURCE_DIR}/src/bitstamp/*.cpp)
file(GLOB_RECURSE KRAKEN_SOURCES ${PROJECT_SOURCE_DIR}/src/kraken/*.cpp)

set (COMMON_SOURCES ${BITSTAMP_SOURCES} ${KRAKEN_SOURCES})

add_subdirectory(tests)
add_subdirectory(doxygen)

configure_file(${PROJECT_SOURCE_DIR}/src/version.h.in ${PROJECT_SOURCE_DIR}/src/version.h @ONLY)
