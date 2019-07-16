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


set(LIBTESTS_SOURCES
  tests.h memory.c misc.c refmpf.c refmpn.c refmpq.c refmpz.c spinner.c trace.c)
if(SIZEOF_VOID_P EQUAL 8)
  set(LIBTESTS_SOURCES ${LIBTESTS_SOURCES} amd64check.c)
else()
  set(LIBTESTS_SOURCES ${LIBTESTS_SOURCES} x86check.c)
endif()
add_library(libtests STATIC ${LIBTESTS_SOURCES})

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

set(TESTS
  t-bswap t-constants t-count_zeros t-hightomask t-modlinv t-popc t-parity t-sub)
foreach(test ${TESTS})
  add_executable(${test} ${test}.c)
  target_link_libraries(${test} libgmp libtests)
  add_test(${test} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${test})
endforeach()

add_subdirectory(cxx)
add_subdirectory(misc)
add_subdirectory(mpf)
add_subdirectory(mpn)
add_subdirectory(mpq)
add_subdirectory(mpz)
