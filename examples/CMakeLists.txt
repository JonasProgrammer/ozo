if (${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU" OR ${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang")
    # both gcc and clang support these warning options
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wsign-compare -pedantic -Werror")
endif(${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU" OR ${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang")

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    # but these options are clang-specific
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-ignored-optimization-argument")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-gnu-string-literal-operator-template")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-gnu-zero-variadic-macro-arguments")
endif("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")

find_program(CCACHE_FOUND ccache)

if(CCACHE_FOUND)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ccache)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ccache)
endif()

add_executable(ozo_request request.cpp)
target_link_libraries(ozo_request ozo)

add_executable(ozo_retry_request retry_request.cpp)
target_link_libraries(ozo_retry_request ozo)

add_executable(ozo_connection_pool connection_pool.cpp)
target_link_libraries(ozo_connection_pool ozo)
