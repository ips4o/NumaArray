###############################################################################
# CMakeLists.txt
#
# Copyright (C) 2020 Michael Axtmann <michael.axtmann@kit.edu>
#
# All rights reserved. Published under the BSD-2 license.
##############################################################################

 cmake_minimum_required(VERSION 3.10)

# Topo
file(GLOB TOPO_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
  extern/Topo/include/*.h
  extern/Topo/source/*.cpp
 )

add_library(topo STATIC ${TOPO_SRCS})
set_target_properties(topo PROPERTIES LINKER_LANGUAGE CXX)
target_include_directories(topo PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/extern/Topo/include)
target_link_libraries(topo PRIVATE hwloc)


# Silo
file(GLOB SILO_SRCS_GENERIC RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
  extern/Silo/include/*.h
  extern/Silo/include/silo/*.h
  extern/Silo/source/consume.cpp
  extern/Silo/source/pointermap.cpp
  extern/Silo/source/osmemory.cpp
  extern/Silo/source/silo.cpp
 )

if (WIN32)
 set(SILO_SRCS ${SILO_SRCS_GENERIC} extern/Silo/source/osmemory-windows.cpp)
else()
 set(SILO_SRCS ${SILO_SRCS_GENERIC} extern/Silo/source/osmemory-linux.cpp)
endif()

add_library(silo STATIC ${SILO_SRCS})
set_target_properties(silo PROPERTIES LINKER_LANGUAGE CXX)
target_include_directories(silo PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/extern/Silo/include/silo ${CMAKE_CURRENT_SOURCE_DIR}/extern/Silo/include)
target_link_libraries(silo PRIVATE topo numa)

# numa_array
file(GLOB NUMA_ARRAY_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
  include/numa_array.hpp
 )

add_library(numa_array STATIC ${NUMA_ARRAY_SRCS})
set_target_properties(numa_array PROPERTIES LINKER_LANGUAGE CXX)
target_include_directories(numa_array PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(numa_array PUBLIC topo silo)
