#Specify the version being used aswell as the language
cmake_minimum_required(VERSION 2.8)
#SET(CMAKE_C_COMPILER /usr/gcc-5.1.0/bin/gcc-5.1.0)
#SET(CMAKE_CXX_COMPILER /usr/gcc-5.1.0/bin/g++-5.1.0)
set(CMAKE_Fortran_COMPILER gfortran)
#Name your project here
project(txcorr)

# add directories to the end of the directory include paths
include_directories("include" ${CMAKE_CURRENT_SOURCE_DIR} PIVlib/include /usr/local/include)
# for hsf5 path
# Your-external "mylib"
#add_library( hdf5 SHARED IMPORTED )
# You can define two import-locations: one for debug and one for release.
#set_target_properties( hdf5 PROPERTIES IMPORTED_LOCATION /usr/local/lib )
# for the PIV lib
add_subdirectory("PIVlib")


# Boost options
FIND_PACKAGE(Boost)
IF (Boost_FOUND)
    INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIR})
    ADD_DEFINITIONS( "-DHAS_BOOST" )
ENDIF()
SET( CMAKE_CXX_FLAGS -std=c++14)
#set(BOOST_ROOT "/usr/local/boost")
#find_package( Boost lexical_cast property_tree)
#include_directories( ${Boost_INCLUDE_DIRS} )
#SET( CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} -L/usr/local/lib -L/usr/gcc-5.1.0/lib -L/opt/intel/composer_xe_2015.3.187/compiler/lib/ -lifcore -lifcoremt -limf -lgfortran -lhdf5 -lhdf5_cpp")
SET( CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} -L/usr/gcc-5.1.0/lib -L/usr/local/lib -lgfortran -lhdf5 -lhdf5_cpp -lfftw3")
link_directories(/usr/local/lib)
add_subdirectory(src)

set_target_properties( txcorr PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin )
