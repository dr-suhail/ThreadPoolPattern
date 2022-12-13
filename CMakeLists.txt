#
# factorize2/CMakeLists.txt --
#

cmake_minimum_required(VERSION 3.1)

project(FACTORIZE2 C)
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)
if(CMAKE_C_COMPILER_ID MATCHES "Clang|GNU")
  add_definitions(-Wall -Wextra -pedantic -Werror)
endif()
INCLUDE(FindThreads)

add_executable(factorize factorize.c)
target_link_libraries(factorize ${CMAKE_THREAD_LIBS_INIT})
