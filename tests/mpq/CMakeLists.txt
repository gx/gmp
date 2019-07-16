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
  t-aors t-cmp t-cmp_ui t-cmp_si t-equal t-get_d t-get_str
  t-inp_str t-inv t-md_2exp t-set_f t-set_str io reuse t-cmp_z)
foreach(test ${TESTS})
  add_executable(mpq-${test} ${test}.c)
  target_link_libraries(mpq-${test} libgmp libtests)
  add_test(mpq-${test} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/mpq-${test})
endforeach()
