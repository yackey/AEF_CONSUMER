
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
#conan_basic_setup(TARGETS)
#conan_basic_setup(TARGETS NO_OUTPUT_DIRS)
conan_basic_setup(NO_OUTPUT_DIRS)

# example how to set c++ compiler flags for GNU
if(CMAKE_CXX_COMPILER_ID MATCHES GNU)
    set(CMAKE_CXX_FLAGS         "${CMAKE_CXX_FLAGS} -DLINUX=1 -O0 -g -Wall -fprofile-arcs -ftest-coverage -Wno-unknown-pragmas -Wno-sign-compare -Woverloaded-virtual -Wwrite-strings -Wno-unused")

    set(CMAKE_CXX_FLAGS_DEBUG   "-O0 -g3 -fPIC -MMD -MP")
    set(CMAKE_CXX_FLAGS_RELEASE "-O0 -g3 -fPIC -MMD -MP")

#    set(CMAKE_CXX_FLAGS_DEBUG   "-O0 -g3 -Wall -c -fmessage-length=0 -fPIC -MMD -MP")
#    set(CMAKE_CXX_FLAGS_RELEASE "-O0 -g3 -Wall -c -fmessage-length=0 -fPIC -MMD -MP")

#    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS_DEBUG} ${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
endif()

message(STATUS "AEF_CONSUMER ${CMAKE_TARGET_PROCESSOR}")
message(STATUS "AEF_CONSUMER ${CMAKE_CXX_COMPILER_ID}")
message(STATUS "AEF_CONSUMER ${CMAKE_C_COMPILER}")
message(STATUS "AEF_CONSUMER ${CMAKE_CXX_COMPILER}")
message(STATUS "AEF_CONSUMER ${CMAKE_CC_COMPILER}")

message(STATUS "AEF_CONSUMER(flags) ${CMAKE_CXX_FLAGS}")

# build executable
#add_executable(hello.x src/hello.F90)

# location of header files
include_directories(
    ${PROJECT_SOURCE_DIR}
    )

# unit test executable
add_executable(
    AefLibConsumerTest
    Main.cpp
    MutexTests.cpp
    TimerTests.cpp
    )

target_link_libraries(
   AefLibConsumerTest
   ${CONAN_LIBS}
#   CONAN_PKG::AEFLib
#   CONAN_PKG::HelloTime
   pthread
   rt
   gcov
   )

# set cdash buildname
set(BUILDNAME
    "${CMAKE_SYSTEM_NAME}-${CMAKE_HOST_SYSTEM_PROCESSOR}-${CMAKE_Fortran_COMPILER_ID}-${cmake_build_type_tolower}"
    CACHE STRING
    "Name of build on the dashboard"
    )

#install(TARGETS AefTest RUNTIME DESTINATION ${AVTEC_BASE}/filesystem/opt)

