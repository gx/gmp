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


function(copy_files)
  math(EXPR n ${ARGC}-1)
  foreach(index1 RANGE 0 ${n} 2)
    math(EXPR index2 "${index1}+1")
    list(GET ARGV ${index1} file_name)
    list(GET ARGV ${index2} func_name)
    add_custom_command(
      OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/generic/${func_name}.c
      COMMAND
        ${CMAKE_COMMAND} -E copy
        ${CMAKE_CURRENT_SOURCE_DIR}/generic/${file_name}.c
        ${CMAKE_CURRENT_BINARY_DIR}/generic/${func_name}.c)
    set_property(
      SOURCE ${CMAKE_CURRENT_BINARY_DIR}/generic/${func_name}.c
      APPEND PROPERTY COMPILE_DEFINITIONS OPERATION_${func_name})
endforeach()
endfunction()

set(map
  logops_n    and_n
  logops_n    andn_n
  logops_n    nand_n
  logops_n    ior_n
  logops_n    iorn_n
  logops_n    nior_n
  logops_n    xor_n
  logops_n    xnor_n
  popham      popcount
  popham      hamdist
  sec_aors_1  sec_add_1
  sec_aors_1  sec_sub_1
  sec_div     sec_div_qr
  sec_div     sec_div_r
  sec_pi1_div sec_pi1_div_qr
  sec_pi1_div sec_pi1_div_r
)

copy_files(${map})

set(
  LIBMPN_SOURCES
  #generic/add.c # inlined
  generic/addmul_1.c
  #generic/add_1.c # inlined
  generic/add_err1_n.c
  generic/add_err2_n.c
  generic/add_err3_n.c
  ${CMAKE_CURRENT_SOURCE_DIR}/generic/add_n.c
  generic/add_n_sub_n.c
  generic/bdiv_dbm1c.c
  generic/bdiv_q.c
  generic/bdiv_qr.c
  generic/bdiv_q_1.c
  generic/binvert.c
  generic/broot.c
  generic/brootinv.c
  generic/bsqrt.c
  generic/bsqrtinv.c
  #generic/cmp.c # inlined
  generic/cnd_add_n.c
  generic/cnd_sub_n.c
  generic/cnd_swap.c
  generic/com.c
  generic/comb_tables.c
  generic/copyd.c
  generic/copyi.c
  generic/dcpi1_bdiv_q.c
  generic/dcpi1_bdiv_qr.c
  generic/dcpi1_divappr_q.c
  generic/dcpi1_div_q.c
  generic/dcpi1_div_qr.c
  generic/diveby3.c
  generic/divexact.c
  generic/dive_1.c
  generic/divis.c
  generic/divrem.c
  generic/divrem_1.c
  generic/divrem_2.c
  generic/div_q.c
  generic/div_qr_1.c
  generic/div_qr_1n_pi1.c
  generic/div_qr_1n_pi2.c
  generic/div_qr_1u_pi2.c
  generic/div_qr_2.c
  generic/div_qr_2n_pi1.c
  generic/div_qr_2u_pi1.c
  generic/dump.c
  generic/fib2_ui.c
  generic/gcd.c
  generic/gcdext.c
  generic/gcdext_1.c
  generic/gcdext_lehmer.c
  generic/gcd_1.c
  generic/gcd_subdiv_step.c
  generic/get_d.c
  generic/get_str.c
  generic/gmp-mparam.h
  generic/hgcd.c
  generic/hgcd2.c
  generic/hgcd2_jacobi.c
  generic/hgcd_appr.c
  generic/hgcd_jacobi.c
  generic/hgcd_matrix.c
  generic/hgcd_reduce.c
  generic/hgcd_step.c
  generic/invert.c
  generic/invertappr.c
  generic/jacbase.c
  generic/jacobi.c
  generic/jacobi_2.c
  generic/lshift.c
  generic/lshiftc.c
  generic/matrix22_mul.c
  generic/matrix22_mul1_inverse_vector.c
  generic/mode1o.c
  generic/mod_1.c
  generic/mod_1_1.c
  generic/mod_1_2.c
  generic/mod_1_3.c
  generic/mod_1_4.c
  generic/mod_34lsub1.c
  generic/mul.c
  generic/mullo_basecase.c
  generic/mullo_n.c
  generic/mulmid.c
  generic/mulmid_basecase.c
  generic/mulmid_n.c
  generic/mulmod_bnm1.c
  generic/mul_1.c
  generic/mul_basecase.c
  generic/mul_fft.c
  generic/mul_n.c
  generic/mu_bdiv_q.c
  generic/mu_bdiv_qr.c
  generic/mu_divappr_q.c
  generic/mu_div_q.c
  generic/mu_div_qr.c
  #generic/neg.c # inlined
  generic/nussbaumer_mul.c
  generic/perfpow.c
  generic/perfsqr.c
  generic/powlo.c
  generic/powm.c
  generic/pow_1.c
  generic/pre_divrem_1.c
  generic/pre_mod_1.c
  generic/random.c
  generic/random2.c
  generic/redc_1.c
  generic/redc_2.c
  generic/redc_n.c
  generic/remove.c
  generic/rootrem.c
  generic/rshift.c
  generic/sbpi1_bdiv_q.c
  generic/sbpi1_bdiv_qr.c
  generic/sbpi1_divappr_q.c
  generic/sbpi1_div_q.c
  generic/sbpi1_div_qr.c
  generic/scan0.c
  generic/scan1.c
  generic/sec_invert.c
  generic/sec_mul.c
  generic/sec_powm.c
  generic/sec_sqr.c
  generic/sec_tabselect.c
  generic/set_str.c
  generic/sizeinbase.c
  generic/sqr.c
  generic/sqrlo.c
  generic/sqrlo_basecase.c
  generic/sqrmod_bnm1.c
  generic/sqrtrem.c
  generic/sqr_basecase.c
  #generic/sub.c # inlined
  generic/submul_1.c
  #generic/sub_1.c # inlined
  generic/sub_err1_n.c
  generic/sub_err2_n.c
  generic/sub_err3_n.c
  generic/sub_n.c
  generic/tdiv_qr.c
  generic/toom22_mul.c
  generic/toom2_sqr.c
  generic/toom32_mul.c
  generic/toom33_mul.c
  generic/toom3_sqr.c
  generic/toom42_mul.c
  generic/toom42_mulmid.c
  generic/toom43_mul.c
  generic/toom44_mul.c
  generic/toom4_sqr.c
  generic/toom52_mul.c
  generic/toom53_mul.c
  generic/toom54_mul.c
  generic/toom62_mul.c
  generic/toom63_mul.c
  generic/toom6h_mul.c
  generic/toom6_sqr.c
  generic/toom8h_mul.c
  generic/toom8_sqr.c
  generic/toom_couple_handling.c
  generic/toom_eval_dgr3_pm1.c
  generic/toom_eval_dgr3_pm2.c
  generic/toom_eval_pm1.c
  generic/toom_eval_pm2.c
  generic/toom_eval_pm2exp.c
  generic/toom_eval_pm2rexp.c
  generic/toom_interpolate_12pts.c
  generic/toom_interpolate_16pts.c
  generic/toom_interpolate_5pts.c
  generic/toom_interpolate_6pts.c
  generic/toom_interpolate_7pts.c
  generic/toom_interpolate_8pts.c
  generic/trialdiv.c
  #generic/udiv_w_sdiv.c
  generic/zero.c
  #generic/zero_p.c # inlined
  ${CMAKE_CURRENT_BINARY_DIR}/fib_table.c
  ${CMAKE_CURRENT_BINARY_DIR}/mp_bases.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/and_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/andn_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/nand_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/ior_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/iorn_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/nior_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/xor_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/xnor_n.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/popcount.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/hamdist.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/sec_add_1.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/sec_sub_1.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/sec_div_qr.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/sec_div_r.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/sec_pi1_div_qr.c
  ${CMAKE_CURRENT_BINARY_DIR}/generic/sec_pi1_div_r.c)
add_library(libmpn OBJECT ${LIBMPN_SOURCES})
add_dependencies(libmpn gen-fib-table gen-mp-bases)
set_property(TARGET libmpn APPEND PROPERTY COMPILE_DEFINITIONS __GMP_WITHIN_GMP)
set_source_files_properties(
  ${CMAKE_CURRENT_BINARY_DIR}/fib_table.c PROPERTIES GENERATED TRUE)
set_source_files_properties(
  ${CMAKE_CURRENT_BINARY_DIR}/mp_bases.c PROPERTIES GENERATED TRUE)
add_dependencies(libmpn
  gen-fac-table gen-fib-table gen-mp-bases
  gen-trialdivtab-h gen-mpn-jacobitab gen-mpn-perfsqr)
