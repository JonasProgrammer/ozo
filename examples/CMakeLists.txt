if (${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU" OR ${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang")
    # both gcc and clang support these warning options
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wsign-compare -pedantic -Werror")
endif(${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU" OR ${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang")

include(CheckCXXCompilerFlag)
check_cxx_compiler_flag("-Wno-ignored-optimization-argument" HAS_WNO_IGNORED_OPT_ARGUMENT)
if(${HAS_WNO_IGNORED_OPT_ARGUMENT})
    target_compile_options(ozo_tests PRIVATE "-Wno-ignored-optimization-argument")
endif()

check_cxx_compiler_flag("-Wno-gnu-string-literal-operator-template" HAS_WNO_GNU_LITERAL)
if(${HAS_NO_GNU_LITERAL})
    target_compile_options(ozo_tests PRIVATE "-Wno-gnu-string-literal-operator-template")
endif()

check_cxx_compiler_flag("-Wno-gnu-zero-variadic-macro-arguments" HAS_WNO_GNU_ZERO_VARIADIC)
if(${HAS_WNO_GNU_ZERO_VARIADIC})
    target_compile_options(ozo_tests PRIVATE "-Wno-gnu-zero-variadic-macro-arguments")
endif()

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
