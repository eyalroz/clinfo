cmake_minimum_required(VERSION 3.26)
project(clinfo
	DESCRIPTION "Utility for listing the information about OpenCL devices"
	HOMEPAGE_URL "https://github.com/Oblomov/clinfo"
	LANGUAGES C CUDA)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")
include(CompileWithWarnings)


#Compile and install a man page, man1/clinfo.1

#find_package(OpenCL REQUIRED)
find_package(CUDAToolkit REQUIRED)
find_package(Threads REQUIRED)

# if an OpenCL-Headers directory exists - add it to the include dirs

add_executable(clinfo src/clinfo.c) 
set_target_properties(clinfo
	PROPERTIES
	C_STANDARD          99
	C_STANDARD_REQUIRED YES
	C_EXTENSIONS        NO
)
target_link_libraries(clinfo CUDA::OpenCL Threads::Threads ${CMAKE_DL_LIBS})
#SPARSE ?= sparse
#SPARSEFLAGS=-Wsparse-all -Wno-decl

