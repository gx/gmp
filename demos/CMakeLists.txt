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


add_subdirectory(expr)

add_executable(factorize factorize.c)
target_link_libraries(factorize libgmp)

add_executable(isprime isprime.c)
target_link_libraries(isprime libgmp)

add_executable(qcn qcn.c)
target_link_libraries(qcn libgmp)

add_executable(primes primes.h primes.c)
target_link_libraries(primes libgmp)

if(NOT WIN32)
  configure_file(pexpr-config.h.in pexpr-config.h)
  add_executable(pexpr pexpr.c)
  target_link_libraries(pexpr libgmp)
  set_target_properties(pexpr PROPERTIES
    INCLUDE_DIRECTORIES ${CMAKE_CURRENT_BINARY_DIR})
endif()
