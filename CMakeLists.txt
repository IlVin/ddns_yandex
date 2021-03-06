cmake_minimum_required(VERSION 2.6.0 FATAL_ERROR)

# DEBUG|RELEASE|RELWITHDEBINFO|MINSIZEREL
# SET (CMAKE_BUILD_TYPE DEBUG)
set (CMAKE_BUILD_TYPE DEBUG)

# Application name
set (PROJECT ddns_yandex)

# Project name
project (${PROJECT})

# Compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")

# Directories
set (SRC_DIR ${PROJECT_SOURCE_DIR}/src)
set (INC_DIR ${PROJECT_SOURCE_DIR}/src)
set (LIB_DIR ${PROJECT_SOURCE_DIR}/lib)
set (TST_DIR ${PROJECT_SOURCE_DIR}/test)



######  common  ############
add_library (common_lib STATIC "${SRC_DIR}/common.cpp")
set (LIBRARIES ${LIBRARIES} common_lib)
include_directories (${INC_DIR})
###### /common  ############



######  BOOST  ############
set (Boost_USE_STATIC_LIBS ON)
set (Boost_USE_MULTITHREADED ON)

find_package (Boost COMPONENTS unit_test_framework REQUIRED)
set (LIBRARIES ${LIBRARIES} ${Boost_LIBRARIES})
include_directories (${Boost_INCLUDE_DIRS})
###### /BOOST  ############



######  CURL  ############
find_library(CURL_LIBRARY NAMES curl)
set (LIBRARIES ${LIBRARIES} ${CURL_LIBRARY})
###### /CURL  ############



###### TESTS  ############
set (TESTS_PROJECT "${PROJECT}_ut")
enable_testing ()
###### /TESTS  ############



######  EXECUTABLE  ############
add_executable (${PROJECT} "${PROJECT_SOURCE_DIR}/main.cpp")
add_executable (${TESTS_PROJECT} "${SRC_DIR}/common_ut.cpp")
###### /EXECUTABLE  ############



######  LINKING LIBRARY  ############
foreach (LIBRARY ${LIBRARIES})
    target_link_libraries(${TESTS_PROJECT} ${LIBRARY})
    target_link_libraries(${PROJECT} ${LIBRARY})
endforeach ()
###### /LINKING LIBRARY  ############





