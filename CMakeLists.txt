cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
cmake_policy(VERSION 3.2)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(MIROIR_VERSION_MAJOR 1)
set(MIROIR_VERSION_MINOR 0)
set(MIROIR_VERSION_PATCH 0)
set(MIROIR_VERSION_REVISION 0)

set(MIROIR_VERSION ${MIROIR_VERSION_MAJOR}.${MIROIR_VERSION_MINOR}.${MIROIR_VERSION_PATCH}.${MIROIR_VERSION_REVISION})
project(miroir VERSION ${MIROIR_VERSION} LANGUAGES CXX)
message(STATUS "miroir: Version " ${MIROIR_VERSION})

add_library(miroir INTERFACE)

include(GNUInstallDirs)

target_include_directories(miroir INTERFACE
	$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/lib/src>
	$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

install(TARGETS miroir EXPORT miroir)

add_library(miroir::miroir ALIAS miroir)

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})
	include(CPack)

	install(DIRECTORY lib/src/miroir DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
	install(EXPORT miroir FILE miroirConfig.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/miroir NAMESPACE miroir::)
    
    include(CMakePackageConfigHelpers)
    write_basic_package_version_file("miroirConfigVersion.cmake" COMPATIBILITY AnyNewerVersion)

	install(
        FILES 
        ${CMAKE_CURRENT_BINARY_DIR}/miroirConfigVersion.cmake 
        DESTINATION 
        ${CMAKE_INSTALL_LIBDIR}/cmake/miroir
    )
endif(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})
