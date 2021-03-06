# Copyright (c) 2007-2013 Hartmut Kaiser
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

include(HPX_AddLibrary)

hpx_option(HPX_HAVE_COMPRESSION_SNAPPY BOOL "Enable snappy compression for parcel data (default: OFF)." OFF ADVANCED)
if(HPX_HAVE_COMPRESSION_SNAPPY)
  find_package(HPX_Snappy)
endif()

macro(add_snappy_module)
  hpx_debug("add_snappy_module" "SNAPPY_FOUND: ${SNAPPY_FOUND}")
  if(HPX_HAVE_COMPRESSION_SNAPPY AND SNAPPY_FOUND)
    hpx_include_sys_directories("${SNAPPY_INCLUDE_DIR}")
    hpx_link_sys_directories("${SNAPPY_LIBRARY_DIR}")

    add_hpx_library(compress_snappy
      SOURCES "${hpx_SOURCE_DIR}/plugins/binary_filter/snappy/snappy_serialization_filter.cpp"
      HEADERS "${hpx_SOURCE_DIR}/hpx/plugins/binary_filter/snappy_serialization_filter.hpp"
      FOLDER "Core/Plugins/Compression"
      DEPENDENCIES "${SNAPPY_LIBRARY}")

    set_property(TARGET compress_snappy_lib APPEND
      PROPERTY COMPILE_DEFINITIONS
      "HPX_LIBRARY_EXPORTS"
      "HPX_PLUGIN_NAME=compress_snappy")

    add_hpx_pseudo_dependencies(plugins.compression.snappy compress_snappy_lib)

    if(NOT HPX_NO_INSTALL)
      hpx_library_install(compress_snappy_lib lib/hpx)
    endif()
  endif()
endmacro()

