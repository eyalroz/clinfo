cmake_minimum_required(VERSION 3.26)
project(clinfo
	DESCRIPTION "Utility for listing the information about OpenCL devices"
	HOMEPAGE_URL "https://github.com/Oblomov/clinfo"
	LANGUAGES C CUDA)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")
include(CompileWithWarnings)
include(GNUInstallDirs)

#Compile and install a man page, man1/clinfo.1

set(PREFER_CUDA_OPENCL_LIB ON CACHE BOOL "Prefer building against CUDA's OpenCL library rather than the system's" )
find_package(CUDAToolkit)
find_package(OpenCL)
find_package(Threads REQUIRED)

# if an OpenCL-Headers directory exists - add it to the include dirs

add_executable(clinfo src/clinfo.c) 
set_target_properties(clinfo
	PROPERTIES
	C_STANDARD          99
	C_STANDARD_REQUIRED YES
	C_EXTENSIONS        NO
)
target_link_libraries(clinfo 
	CUDA::OpenCL
	Threads::Threads
	${CMAKE_DL_LIBS}
)

include(GNUInstallDirs)
file(ARCHIVE_CREATE OUTPUT foo.1.gz PATHS doc/foo.1 FORMAT raw COMPRESSION GZip)
install(FILES foo.1.gz DESTINATION "${CMAKE_INSTALL_MANDIR}")
