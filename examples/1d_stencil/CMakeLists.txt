# Copyright (c) 2014 Hartmut Kaiser
# Copyright (c) 2011 Bryce Lelbach
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

set(example_programs
    1d_stencil_1
    1d_stencil_2
    1d_stencil_3
    1d_stencil_4
    1d_stencil_5
    1d_stencil_6
    1d_stencil_7)

foreach(example_program ${example_programs})

  set(${example_program}_FLAGS DEPENDENCIES iostreams_component)

  set(sources ${example_program}.cpp)

  source_group("Source Files" FILES ${sources})

  # add example executable
  add_hpx_executable(${example_program}
                     SOURCES ${sources}
                     ${${example_program}_FLAGS}
                     FOLDER "Examples/1D Stencil/${example_program}")

  # add a custom target for this example
  add_hpx_pseudo_target(examples.1d_stencil.${example_program})

  # make pseudo-targets depend on master pseudo-target
  add_hpx_pseudo_dependencies(examples.1d_stencil
                              examples.1d_stencil.${example_program})

  # add dependencies to pseudo-target
  add_hpx_pseudo_dependencies(examples.1d_stencil.${example_program}
                              ${example_program}_exe)
endforeach()

