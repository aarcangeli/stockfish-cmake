project(HelloWorld)
cmake_minimum_required(VERSION 3.0)

set(CMAKE_CXX_STANDARD 20)

# populate GIT_DATE
execute_process(
    COMMAND git show -s --date=format:%Y%m%d --format=%cd HEAD
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    OUTPUT_VARIABLE GIT_DATE
    OUTPUT_STRIP_TRAILING_WHITESPACE
    RESULT_VARIABLE GIT_RESULT
    ERROR_QUIET
)
if(GIT_RESULT EQUAL 0)
    message(STATUS "Git date: ${GIT_DATE}")
    add_definitions(-DGIT_DATE=${GIT_DATE})
endif()

# populate GIT_SHA
execute_process(
    COMMAND git rev-parse HEAD
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    OUTPUT_VARIABLE GIT_SHA
    OUTPUT_STRIP_TRAILING_WHITESPACE
    RESULT_VARIABLE GIT_RESULT
    ERROR_QUIET
)
if(GIT_RESULT EQUAL 0)
    message(STATUS "Git SHA: ${GIT_SHA}")
    add_definitions(-DGIT_SHA=${GIT_SHA})
endif()

# glob src directory
file(GLOB_RECURSE SRC_FILES src/*.cpp src/*.h)
add_executable(stockfish ${SRC_FILES})
