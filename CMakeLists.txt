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


cmake_minimum_required(VERSION 3.12.2)

project(GMP
  VERSION 6.1.2
  DESCRIPTION "GNU Multiple Precision Arithmetic Library"
  HOMEPAGE_URL "https://gmplib.org"
  LANGUAGES C CXX ASM)

set(LIBGMP_LT_CURRENT 13)
set(LIBGMP_LT_REVISION 2)
set(LIBGMP_LT_AGE 3)

set(LIBGMPXX_LT_CURRENT 9)
set(LIBGMPXX_LT_REVISION 2)
set(LIBGMPXX_LT_AGE 5)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Use solution folders for Visual Studio
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# Package information

# Set the name of this package.
set(PACKAGE "gmp")
# Set the full name of this package.
set(PACKAGE_NAME "GNU MP")
# Set the one symbol short name of this package.
set(PACKAGE_TARNAME "gmp")
# Set the version of this package.
set(PACKAGE_VERSION "${GMP_VERSION}")
# Set the full name and version of this package.
set(PACKAGE_STRING "${PACKAGE_NAME} ${GMP_VERSION}")
# Set the home page for this package.
set(PACKAGE_URL "http://www.gnu.org/software/gmp/")
# Set the address where bug reports for this package should be sent.
set(PACKAGE_BUGREPORT
  "gmp-bugs@gmplib.org, see https://gmplib.org/manual/Reporting-Bugs.html")

# Set the return type of signal handlers (`int' or `void').
set(RETSIGTYPE void)

# Disable the use of inline assembly.
set(NO_ASM 1)

set(VERSION ${GMP_VERSION})

include(CheckCSourceCompiles)
include(CheckIncludeFile)
include(CheckIncludeFiles)
include(CheckIncludeFileCXX)
include(CheckFunctionExists)
include(CheckCXXSymbolExists)
include(CheckSymbolExists)
include(CheckTypeSize)
include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

option(WANT_ASSERT "Enable ASSERT checking" OFF)
option(WANT_FFT "Enable FFTs for multiplication" ON)
option(WANT_OLD_FFT_FULL "Enable old mpn_mul_fft_full for multiplication" OFF)
option(WANT_FAKE_CPUID "Enable GMP_CPU_TYPE faking cpuid" OFF)
option(WANT_FAT_BINARY "Enable fat binary" OFF)
option(WANT_CXX "Enable C++ wrapper" ON)

if(SIZEOF_VOID_P EQUAL 8)
  set(GMP_MPARAM_H_SUGGEST ${CMAKE_SOURCE_DIR}/mpn/x86_64/k8/gmp-mparam.h)
else()
  set(GMP_MPARAM_H_SUGGEST ${CMAKE_SOURCE_DIR}/mpn/x86/gmp-mparam.h)
endif()
set(WANT_TMP_ALLOCA 1)

set(HAVE_HOST_CPU_FAMILY_power 0)
set(HAVE_HOST_CPU_FAMILY_powerpc 0)
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(GMP_LIMB_BITS 64)
  set(DEFN_LONG_LONG_LIMB "#define _LONG_LONG_LIMB 1")
  set(HOST_DOS64 1)
else()
  set(GMP_LIMB_BITS 32)
  set(DEFN_LONG_LONG_LIMB "")
endif()
set(GMP_NAIL_BITS 0 CACHE STRING "Use nails on limbs")
set(LIBGMP_DLL 1)
set(CC ${CMAKE_C_COMPILER})
set(CFLAG ${CMAKE_C_FLAGS})

configure_file(gmp-h.in gmp.h @ONLY)

# Check whether gcc __attribute__ ((const)) works.
check_c_source_compiles(
  "int foo(int x) __attribute__((const)); int main() {}"
  HAVE_ATTRIBUTE_CONST)

# Check whether gcc __attribute__ ((malloc)) works.
check_c_source_compiles(
  "void *foo(int x) __attribute ((malloc)); int main() {}"
  HAVE_ATTRIBUTE_MALLOC)

# Check whether gcc __attribute__ ((mode (XX))) works.
check_c_source_compiles(
  "tyedef int SItype __attribute__ ((mode (SI))); int main() {}"
  HAVE_ATTRIBUTE_MODE)

# Check whether gcc __attribute__ ((noreturn)) works.
check_c_source_compiles(
  "void foo(int x) __attribute__ ((noreturn)); int main() {}"
  HAVE_ATTRIBUTE_NORETURN)

# Check header files

# Set to 1 if you have the ANSI C header files.
set(STDC_HEADERS 1)
check_include_file(alloca.h HAVE_ALLOCA_H)
check_include_file(dlfcn.h HAVE_DLFCN_H)
check_include_file(fcntl.h HAVE_FCNTL_H)
check_include_file(float.h HAVE_FLOAT_H)
check_include_file(inttypes.h HAVE_INTTYPES_H)
check_include_file(invent.h HAVE_INVENT_H)
check_include_file(langinfo.h HAVE_LANGINFO_H)
check_include_file(locale.h HAVE_LOCALE_H)
check_include_file(machine/hal_sysinfo.h HAVE_MACHINE_HAL_SYSINFO_H)
check_include_file(memory.h HAVE_MEMORY_H)
check_include_file(nl_types.h HAVE_NL_TYPES_H)
check_include_file_cxx(sstream HAVE_SSTREAM)
check_include_file(stdint.h HAVE_STDINT_H)
check_include_file(stdlib.h HAVE_STDLIB_H)
check_include_file(strings.h HAVE_STRINGS_H)
check_include_file(string.h HAVE_STRING_H)
check_include_file(sys/attributes.h HAVE_SYS_ATTRIBUTES_H)
check_include_file(sys/iograph.h HAVE_SYS_IOGRAPH_H)
check_include_file(sys/mman.h HAVE_SYS_MMAN_H)
check_include_file(sys/processor.h HAVE_SYS_PROCESSOR_H)
check_include_file(sys/pstat.h HAVE_SYS_PSTAT_H)
check_include_file(sys/resource.h HAVE_SYS_RESOURCE_H)
check_include_file(sys/stat.h HAVE_SYS_STAT_H)
check_include_file(sys/sysctl.h HAVE_SYS_SYSCTRL_H)
check_include_file(sys/sysinfo.h HAVE_SYS_SYSINFO_H)
check_include_file(sys/syssgi.h HAVE_SYS_SYSSGI_H)
check_include_file(sys/systemcfg.h HAVE_SYS_SYSTEMCFG_H)
check_include_file(sys/times.h HAVE_SYS_TIMES_H)
check_include_file(sys/time.h HAVE_SYS_TIME_H)
check_include_file(sys/types.h HAVE_SYS_TYPES_H)
check_include_file(unistd.h HAVE_UNISTD_H)
check_include_files("sys/time.h;time.h" TIME_WITH_SYS_TIME)

# Check functions
check_function_exists(alarm HAVE_ALARM)
check_function_exists(alloca HAVE_ALLOCA)
check_function_exists(attr_get HAVE_ATTR_GET)
check_function_exists(clock HAVE_CLOCK)
check_function_exists(clock_gettime HAVE_CLOCK_GETTIME)
check_function_exists(cputime HAVE_CPUTIME)
check_function_exists(getpagesize HAVE_GETPAGESIZE)
check_function_exists(getrusage HAVE_GETRUSAGE)
check_function_exists(getsysinfo HAVE_GETSYSINFO)
check_function_exists(gettimeofday HAVE_GETTIMEOFDAY)
check_function_exists(localeconv HAVE_LOCALECONV)
check_function_exists(memset HAVE_MEMSET)
check_function_exists(mmap HAVE_MMAP)
check_function_exists(mprotect HAVE_MPROTECT)
check_function_exists(nl_langinfo HAVE_NL_LANGINFO)
check_function_exists(obstack_vprintf HAVE_OBSTACK_VPRINTF)
check_function_exists(popen HAVE_POPEN)
check_function_exists(processor_info HAVE_PROCESSOR_INFO)
check_function_exists(pstat_getprocessor HAVE_PSTAT_GETPROCESSOR)
check_function_exists(raise HAVE_RAISE)
check_function_exists(read_real_time HAVE_READ_REAL_TIME)
check_function_exists(sigaction HAVE_SIGACTION)
check_function_exists(sigaltstack HAVE_SIGALTSTACK)
check_function_exists(sigstack HAVE_SIGSTACK)
check_function_exists(strchr HAVE_STRCHR)
check_function_exists(strerror HAVE_STRERROR)
check_function_exists(strnlen HAVE_STRNLEN)
check_function_exists(strtol HAVE_STRTOL)
check_function_exists(strtoul HAVE_STRTOUL)
check_function_exists(sysctl HAVE_SYSCTL)
check_function_exists(sysctlbyname HAVE_SYSCTLBYNAME)
check_function_exists(syssgi HAVE_SYSSGI)
check_function_exists(times HAVE_TIMES)

check_symbol_exists(fgetc stdio.h HAVE_DECL_FGETC)
check_symbol_exists(fscanf stdio.h HAVE_DECL_FSCANF)
check_symbol_exists(optarg stdio.h HAVE_DECL_OPTARG)
check_symbol_exists(sys_errlist stdio.h errno.h HAVE_DECL_SYS_ERRLIST)
check_symbol_exists(sys_nerr errno.h HAVE_DECL_SYS_NERR)
check_symbol_exists(ungetc stdio.h HAVE_DECL_UNGETC)
check_symbol_exists(vfprintf stdio.h HAVE_DECL_VFPRINTF)
check_symbol_exists(vsnprintf stdio.h HAVE_VSNPRINTF)
check_symbol_exists("struct pst_processor" sys/pstat.h HAVE_PSP_ITICKSPERCLKTICK)
check_cxx_symbol_exists("std::locale" locale HAVE_STD__LOCALE)

# Check types
check_type_size("void *" SIZEOF_VOID_P)
check_type_size("unsigned short" SIZEOF_UNSIGNED_SHORT)
check_type_size("unsigned" SIZEOF_UNSIGNED)
check_type_size("unsigned long" SIZEOF_UNSIGNED_LONG)
check_type_size("long double" LONG_DOUBLE)
if(HAVE_LONG_DOUBLE)
  set(SIZEOF_LONG_DOUBLE ${LONG_DOUBLE})
endif()
check_type_size("long long" LONG_LONG)
if(HAVE_LONG_LONG)
  set(SIZEOF_LONG_LONG ${LONG_LONG})
endif()
check_type_size("ptrdiff_t" SIZEOF_PTRDIFF_T)
check_type_size("quad_t" SIZEOF_QUAD_T)
check_type_size("uint_least32_t" SIZEOF_UINT_LEAST32_T)
check_type_size("intmax_t" INTMAX_T)
if(HAVE_INTMAX_T)
  set(SIZEOF_INTMAX_T ${INTMAX_T})
endif()
check_type_size("intptr_t" INTPTR_T)
if(HAVE_INTPTR_T)
  set(SIZEOF_INTPTR_T ${INTPTR_T})
endif()
check_type_size("stack_t" SIZEOF_STACK_T)
set(CMAKE_EXTRA_INCLUDE_FILES ${CMAKE_CURRENT_BINARY_DIR}/gmp.h)
check_type_size("mp_limb_t" SIZEOF_MP_LIMB_T)
set(CMAKE_EXTRA_INCLUDE_FILES)

# CPU Family
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(HAVE_HOST_CPU_FAMILY_x86_64 1)
else()
  set(HAVE_HOST_CPU_FAMILY_x86 1)
endif()

# Test endian type.
include(TestBigEndian)
test_big_endian(big_endian)
if(big_endian)
  set(HAVE_DOUBLE_IEEE_BIG_ENDIAN 1)
  set(HAVE_LIMB_BIG_ENDIAN 1)
  set(WORDS_BIGENDIAN 1)
else()
  set(HAVE_DOUBLE_IEEE_LITTLE_ENDIAN 1)
  set(HAVE_LIMB_LITTLE_ENDIAN 1)
endif()

configure_file(config.h.in config.h @ONLY)

if(MSVC)
  add_compile_options(/wd4018 /wd4146 /wd4244 /wd4267 /wd4996)
endif()

add_custom_target(bootstrap DEPENDS bootstrap.c)

# Generate fac_table.h
add_executable(gen-fac gen-fac.c)
add_dependencies(gen-fac bootstrap)
add_custom_command(
  OUTPUT fac_table.h
  COMMAND gen-fac ${GMP_LIMB_BITS} ${GMP_NAIL_BITS} >fac_table.h
  DEPENDS gen-fac
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating fac_table.h ..."
)
add_custom_target(
  gen-fac-table ALL
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/fac_table.h
)

# Generate fib_table.h and mpn/fib_table.c
add_executable(gen-fib gen-fib.c)
add_dependencies(gen-fib bootstrap)
add_custom_command(
  OUTPUT fib_table.h
  COMMAND gen-fib header ${GMP_LIMB_BITS} ${GMP_NAIL_BITS} >fib_table.h
  DEPENDS gen-fib
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating fib_table.h ..."
)
add_custom_command(
  OUTPUT mpn/fib_table.c
  COMMAND gen-fib table ${GMP_LIMB_BITS} ${GMP_NAIL_BITS} >mpn/fib_table.c
  DEPENDS gen-fib
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating mpn/fib_table.c ..."
)
add_custom_target(
  gen-fib-table ALL
  DEPENDS
    ${CMAKE_CURRENT_BINARY_DIR}/fib_table.h
    ${CMAKE_CURRENT_BINARY_DIR}/mpn/fib_table.c
)

# Generate mp_bases.h and mpn/mp_bases.c
add_executable(gen-bases gen-bases.c)
add_dependencies(gen-bases bootstrap)
add_custom_command(
  OUTPUT mp_bases.h
  COMMAND gen-bases header ${GMP_LIMB_BITS} ${GMP_NAIL_BITS} >mp_bases.h
  DEPENDS gen-bases
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating mp_bases.h ..."
)
add_custom_command(
  OUTPUT mpn/mp_bases.c
  COMMAND gen-bases table ${GMP_LIMB_BITS} ${GMP_NAIL_BITS} >mpn/mp_bases.c
  DEPENDS gen-bases
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating mpn/mp_bases.c ..."
)
add_custom_target(
  gen-mp-bases ALL
  DEPENDS
    ${CMAKE_CURRENT_BINARY_DIR}/mp_bases.h
    ${CMAKE_CURRENT_BINARY_DIR}/mpn/mp_bases.c
)

# Generate trialdivtab.h
add_executable(gen-trialdivtab gen-trialdivtab.c)
add_dependencies(gen-trialdivtab bootstrap)
add_custom_command(
  OUTPUT trialdivtab.h
  COMMAND gen-trialdivtab ${GMP_LIMB_BITS} 8000 >trialdivtab.h
  DEPENDS gen-trialdivtab
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating trialdivtab.h ..."
)
add_custom_target(
  gen-trialdivtab-h ALL
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/trialdivtab.h
)

# Generate mpn/jacobitab.h
add_executable(gen-jacobitab gen-jacobitab.c)
add_custom_command(
  OUTPUT mpn/jacobitab.h
  COMMAND gen-jacobitab >mpn/jacobitab.h
  DEPENDS gen-jacobitab
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating mpn/jacobitab.h ..."
)
add_custom_target(
  gen-mpn-jacobitab ALL
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/mpn/jacobitab.h
)

# Generate mpn/perfsqr.h
add_executable(gen-psqr gen-psqr.c)
add_dependencies(gen-psqr bootstrap)
add_custom_command(
  OUTPUT mpn/perfsqr.h
  COMMAND gen-psqr ${GMP_LIMB_BITS} ${GMP_NAIL_BITS} >mpn/perfsqr.h
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMENT "Generating mpn/perfsqr.h ..."
)
add_custom_target(
  gen-mpn-perfsqr ALL
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/mpn/perfsqr.h
)

include_directories(
  ${CMAKE_SOURCE_DIR}
  ${CMAKE_BINARY_DIR}
  ${CMAKE_BINARY_DIR}/mpn
)
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  include_directories(${CMAKE_SOURCE_DIR}/mpn/x86_64/k8)
else()
  include_directories(${CMAKE_SOURCE_DIR}/mpn/x86)
endif()

add_subdirectory(mpn)
add_subdirectory(mpf)
add_subdirectory(mpq)
add_subdirectory(mpz)
add_subdirectory(printf)
add_subdirectory(rand)
add_subdirectory(scanf)

set(
  LIBGMP_SOURCES
  gmp-impl.h longlong.h
  assert.c compat.c errno.c extract-dbl.c invalid.c memory.c
  mp_bpl.c mp_clz_tab.c mp_dv_tab.c mp_minv_tab.c mp_get_fns.c mp_set_fns.c
  version.c nextprime.c primesieve.c
)
set(
  LIBGMP_EXTRA_SOURCES
  tal-reent.c
)

add_library(
  libgmp SHARED
  ${LIBGMP_SOURCES}
  ${LIBGMP_EXTRA_SOURCES}
  $<TARGET_OBJECTS:libmpf>
  $<TARGET_OBJECTS:libmpn>
  $<TARGET_OBJECTS:libmpq>
  $<TARGET_OBJECTS:libmpz>
  $<TARGET_OBJECTS:libprintf>
  $<TARGET_OBJECTS:librandom>
  $<TARGET_OBJECTS:libscanf>
)

target_compile_definitions(libgmp PRIVATE __GMP_WITHIN_GMP)

set_target_properties(
  libgmp
  PROPERTIES
  OUTPUT_NAME libgmp$<$<CONFIG:Debug>:d>-${LIBGMP_LT_CURRENT}
  VERSION ${GMP_VERSION}
  SOVERSION ${LIBGMP_LT_CURRENT})

# Install libgmp.
install(
  TARGETS libgmp
  EXPORT libgmp-export
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# Install libgmp header file.
install(
  FILES ${CMAKE_CURRENT_BINARY_DIR}/gmp.h
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  COMPONENT Development)

# Install libgmp PDB files.
if(MSVC)
  install(
    FILES $<TARGET_PDB_FILE:libgmp>
    DESTINATION ${CMAKE_INSTALL_BINDIR}
    COMPONENT Development
    OPTIONAL)
endif()

set(GMP_CONFIG_PACKAGE_LOCATION ${CMAKE_INSTALL_LIBDIR}/cmake/gmp)

# Create a version file for GMP.
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/GMPConfigVersion.cmake"
  VERSION ${GMP_VERSION}
  COMPATIBILITY AnyNewerVersion)

export(
  EXPORT libgmp-export
  FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/libgmp.cmake"
  NAMESPACE GMP::)

install(EXPORT libgmp-export
  FILE libgmp.cmake
  NAMESPACE GMP::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/gmp)

# Install GMP config-file package.
install(
  FILES
    ${CMAKE_SOURCE_DIR}/cmake/GMPConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/cmake/GMPConfigVersion.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/gmp
  COMPONENT Development)

if(WANT_CXX)
  add_subdirectory(cxx)
  set(LIBGMPXX_SOURCES cxx/dummy.cc)
  add_library(
    libgmpxx SHARED
    ${LIBGMPXX_SOURCES}
    $<TARGET_OBJECTS:libcxx>
  )
  target_compile_definitions(libgmpxx PRIVATE __GMP_WITHIN_GMP)
  target_link_libraries(libgmpxx PUBLIC libgmp)
  set_target_properties(
    libgmpxx PROPERTIES
    OUTPUT_NAME libgmpxx$<$<CONFIG:Debug>:d>-${LIBGMPXX_LT_CURRENT})

  # Install libgmpxx.
  install(
    TARGETS libgmpxx
    EXPORT libgmpxx-export
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

  # Install libgmpxx header file.
  install(
    FILES gmpxx.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    COMPONENT Development)

  # Install libgmpxx PDB files.
  if(MSVC)
    install(
      FILES $<TARGET_PDB_FILE:libgmpxx>
      DESTINATION ${CMAKE_INSTALL_BINDIR}
      COMPONENT Development
      OPTIONAL)
  endif()

  # Create a version file for GMP.
  write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/cmake/GMPXXConfigVersion.cmake"
    VERSION ${GMP_VERSION}
    COMPATIBILITY AnyNewerVersion)

  export(
    EXPORT libgmpxx-export
    FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/libgmpxx.cmake"
    NAMESPACE GMP::)
  install(EXPORT libgmpxx-export
    FILE libgmpxx.cmake
    NAMESPACE GMP::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/gmpxx)

  # Install GMPXX config-file package.
  install(
    FILES
      ${CMAKE_SOURCE_DIR}/cmake/GMPXXConfig.cmake
      ${CMAKE_CURRENT_BINARY_DIR}/cmake/GMPXXConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/gmpxx
    COMPONENT Development)
endif()

add_subdirectory(demos)

include(CTest)
enable_testing()
if(BUILD_TESTING)
  add_subdirectory(tests)
endif()
