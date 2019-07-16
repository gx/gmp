# Copyright 2019 Free Software Foundation, Inc.
#
#  This file is part of the GNU MP Library.
#
#  The GNU MP Library is free software; you can redistribute it and/or modify
#  it under the terms of either:
#
#    * the GNU Lesser General Public License as published by the Free
#      Software Foundation; either version 3 of the License, or (at your
#      option) any later version.
#
#  or
#
#    * the GNU General Public License as published by the Free Software
#      Foundation; either version 2 of the License, or (at your option) any
#      later version.
#
#  or both in parallel, as here.
#
#  The GNU MP Library is distributed in the hope that it will be useful, but
#  WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
#  or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
#  for more details.
#
#  You should have received copies of the GNU General Public License and the
#  GNU Lesser General Public License along with the GNU MP Library.  If not,
#  see https://www.gnu.org/licenses/.


set(LIBEXPR_SOURCES expr.h expr-impl.h
  expr.c exprv.c exprz.c exprza.c exprq.c exprqa.c exprf.c exprfa.c)
add_library(libexpr STATIC ${LIBEXPR_SOURCES})

add_executable(run-expr run-expr.c)
target_link_libraries(run-expr libexpr libgmp)

if(BUILD_TESTING)
  add_executable(t-expr t-expr.c)
  target_link_libraries(t-expr libexpr libgmp libtests)
  set_target_properties(t-expr PROPERTIES
    INCLUDE_DIRECTORIES "${CMAKE_BINARY_DIR};${CMAKE_SOURCE_DIR}/tests")
endif()
