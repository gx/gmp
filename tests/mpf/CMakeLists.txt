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
  t-dm2exp t-conv t-add t-sub t-sqrt t-sqrt_ui t-muldiv reuse
  t-cmp_d t-cmp_si t-div t-fits t-get_d t-get_d_2exp
  t-get_si t-get_ui t-gsprec t-inp_str t-int_p t-mul_ui
  t-set t-set_q t-set_si t-set_ui t-trunc t-ui_div t-eq t-pow_ui)
foreach(test ${TESTS})
  add_executable(mpf-${test} ${test}.c)
  target_link_libraries(mpf-${test} libgmp libtests)
  add_test(mpf-${test} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/mpf-${test})
endforeach()
