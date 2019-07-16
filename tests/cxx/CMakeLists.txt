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


set(TESTS
  t-binary t-cast t-cxx11 t-headers t-iostream t-istream t-locale t-misc t-mix
  t-ops t-ops2 t-ops3 t-ostream t-prec t-ternary t-unary
  t-do-exceptions-work-at-all-with-this-compiler
  t-assign t-constr t-rand)
foreach(test ${TESTS})
  add_executable(cxx-${test} ${test}.cc)
  target_link_libraries(cxx-${test} libgmp libgmpxx libtests)
  add_test(cxx-${test} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/cxx-${test})
endforeach()
