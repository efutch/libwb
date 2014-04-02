############################################################################################
# <summary> CMakeLists.txt for libWB and CUDA.                </summary>
# <date>    2013-01-07                                </date>
# <author>  Tran Minh Quan. edit by Johannes Kast, Michael Sarahan      </author>
# <email>   quantm@unist.ac.kr  kast.jo@googlemail.com msarahan@gmail.com </email>
############################################################################################
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(libwb)

find_package(CUDA REQUIRED)

include_directories(${CMAKE_SOURCE_DIR})

set(SM_COMPUTE_CAPABILITY "sm_12")

set(CUDA_HOST_COMPILER "/usr/bin/clang++")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -stdlib=libstdc++ -isystem ${CUDA_INCLUDE_DIRS}")
set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} --gpu-architecture=${SM_COMPUTE_CAPABILITY} -Xcompiler -stdlib=libstdc++; -Xlinker -stdlib=libstdc++")

file(GLOB wb_header_files *.h)
file(GLOB wb_source_files *.cpp)

set(LIBWB_LINK_LIBRARIES "-lm -framework CUDA")

set(executables mp0 mp1 mp2 mp3 mp4 mp5 mp6)
foreach (executable ${executables})
    string(TOUPPER "${executable}" EXECUTABLE)
    CUDA_ADD_EXECUTABLE(${EXECUTABLE} ${executable}.cu ${wb_header_files} ${wb_source_files})
    target_link_libraries(${EXECUTABLE} "${LIBWB_LINK_LIBRARIES}")
endforeach()
