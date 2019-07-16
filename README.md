# Porting GMP to Windows

This repository ports [GMP](https://gmplib.org) to Windows using CMake
and Microsoft Visual Studio Compilers. Note that the libmpn module
uses the generic C implementation instead of the optimized assembly
code.

## How To Build and Install

```
git clone https://github.com/gx/gmp.git
cd gmp
mkdir build
cd build
cmake -G "Visual Studio 15 2017 Win64" -DCMAKE_INSTALL_PREFIX=<path-to-install> ..
cmake --build . --config Release --target ALL_BUILD
cmake --build . --config Release --target RUN_TESTS
cmake --build . --config Release --target INSTALL
```
