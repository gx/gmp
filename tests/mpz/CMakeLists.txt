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
  t-addsub t-cmp t-mul t-mul_i t-tdiv t-tdiv_ui t-fdiv
  t-fdiv_ui t-cdiv_ui t-gcd t-gcd_ui t-lcm t-invert dive dive_ui t-sqrtrem
  convert io t-inp_str logic bit t-powm t-powm_ui t-pow t-div_2exp
  t-root t-perfsqr t-perfpow t-jac t-bin t-get_d t-get_d_2exp t-get_si
  t-set_d t-set_si
  t-fac_ui t-mfac_uiui t-primorial_ui t-fib_ui t-lucnum_ui t-scan t-fits
  t-divis t-divis_2exp t-cong t-cong_2exp t-sizeinbase t-set_str
  t-aorsmul t-cmp_d t-cmp_si t-hamdist t-oddeven t-popcount t-set_f
  t-io_raw t-import t-export t-pprime_p t-nextprime t-remove t-limbs)
foreach(test ${TESTS})
  add_executable(mpz-${test} ${test}.c)
  target_link_libraries(mpz-${test} libgmp libtests)
  add_test(mpz-${test} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/mpz-${test})
endforeach()
