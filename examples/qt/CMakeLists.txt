# Copyright (c) 2012 Thomas Heller
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

find_package(Qt4)
include(${QT_USE_FILE})
    
if(HPX_CXX11_LAMBDAS AND QT4_FOUND)

    QT4_WRAP_CPP(QT_MOC_SOURCES widget.hpp hpx_qt.hpp OPTIONS -DBOOST_TT_HAS_OPERATOR_HPP_INCLUDED)
    include_directories(./)

  set(qt_sources
      qt.cpp
      widget.cpp
      hpx_qt.cpp
      ${QT_MOC_SOURCES})

  include_directories(QT4_INCLUDE_DIRS})

  # add jacobi executable
  add_hpx_executable(qt
    MODULE qt
    SOURCES ${qt_sources}
    ${qt_FLAGS}
    FOLDER "Examples/Qt")

  target_link_libraries(qt_exe ${QT_LIBRARIES})

  # add a custom target for this example
  add_hpx_pseudo_target(examples.qt.qt)

  # make pseudo-targets depend on master pseudo-target
  add_hpx_pseudo_dependencies(examples.qt
                              examples.qt.qt)

  # add dependencies to pseudo-target
  add_hpx_pseudo_dependencies(examples.qt.qt
                              qt_exe)

endif()