cmake_minimum_required(VERSION 2.8)

# Name of the project (will be the name of the plugin)
project (cryptonote)

# Essential include files to build a node addon,
# you should add this line in every CMake.js based project.
include_directories(${CMAKE_JS_INC})

# Boost dependency
# You need to have Boost built and env vars BOOST_ROOT, BOOST_LIBRARYDIR.
set(Boost_USE_STATIC_LIBS OFF)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)
find_package(Boost 1.58 REQUIRED COMPONENTS date_time system thread filesystem regex program_options)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})
add_definitions(${Boost_LIB_DIAGNOSTIC_DEFINITIONS})

#-------------------------------------------------------------------------------
# Declare the location of the source files
SET (SOURCE_FILES
  src/main.cc
  src/cryptonote_core/cryptonote_format_utils.cpp
  src/cryptonote_core/cryptonote_basic_impl.cpp
  src/crypto/tree-hash.c
  src/crypto/crypto.cpp
  src/crypto/crypto-ops.c
  src/crypto/crypto-ops-data.c
  src/crypto/hash.c
  src/crypto/keccak.c
  src/common/base58.cpp
  src/crypto/slow-hash.c

  src/cryptonote_core/difficulty.cpp
  src/cryptonote_core/miner.cpp
  src/crypto/crypto.cpp
  src/crypto/random.c
  src/crypto/hash-extra-blake.c
  src/crypto/hash-extra-groestl.c
  src/crypto/hash-extra-jh.c
  src/crypto/hash-extra-skein.c
  src/crypto/oaes_lib.c
  src/crypto/blake256.c
  src/crypto/groestl.c
  src/crypto/jh.c
  src/crypto/skein.c
)

# Additional include directories
include_directories(src src/contrib/epee/include)

# definitions
#set(CMAKE_POSITION_INDEPENDENT_CODE ON)
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")

IF(MSVC)
    add_definitions(-D_WIN32_WINNT=0x0601)
ENDIF(MSVC)
#-------------------------------------------------------------------------------

# This line will tell CMake that we're building a shared library
# from the above source files
# named after the project's name
add_library(${PROJECT_NAME} SHARED ${SOURCE_FILES})

# This line will give our library file a .node extension without any "lib" prefix
set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "" SUFFIX ".node")

# Essential library files to link to a node addon,
# you should add this line in every CMake.js based project.
# Define Boost dependencies there.
target_link_libraries(${PROJECT_NAME} ${CMAKE_JS_LIB};${Boost_LIBRARIES})

#add_custom_command(
#  TARGET cryptonote POST_BUILD
#  COMMAND ${CMAKE_COMMAND} -E copy
#    ${CMAKE_CURRENT_BINARY_DIR}/Release/${PROJECT_NAME}.node
#    ../bin/${PROJECT_NAME}.node)
