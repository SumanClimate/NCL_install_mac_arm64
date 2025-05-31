# NCL_install_mac_arm64
This repository guides you how to install NCAR Command Language (NCL) using the source code
NCL (NCAR Command Language) was a very useful tool in the past. Many researchers who began their careers using GrADS gradually shifted to NCL/NCAR Graphics due to its powerful graphical capabilities, user-friendly interface, strong user community, and a wide range of readily available functions. Although NCL is no longer actively maintained, it remains a valuable tool for the atmospheric modeling community, both at the global and regional levels.\
In the past, installing NCL was extremely easy, thanks to the availability of precompiled binaries for various operating systems and compilers—installation typically required nothing more than extracting the archive. With the evolution of macOS, the last few versions of NCL included precompiled binaries compatible with macOS on Intel-based systems, which worked well.\
However, with the advent of Mac systems using M1/M2 (Apple Silicon) and the arm64 architecture, these older precompiled binaries no longer function. As a result, the only way to run NCL on Mac-arm64 systems is to build it from the source—a process that can be quite challenging. This document provides a step-by-step guide to installing NCL on Mac-arm64 systems.
# Prerequisites
Few points to be noted before start of the installation:
1. Installation process will be on MacOS Sonoma version 14.3.1 will be used:\
suman@Sumans-Air:[/Users/suman] uname -a
Darwin Sumans-Air.lan 23.3.0 Darwin Kernel Version 23.3.0: Wed Dec 20 21:30:27 PST 2023; root:xnu-10002.81.5~7/RELEASE_ARM64_T8103 arm64 arm Darwin
2. Following version of gcc/gfortran will be used:\
suman@Sumans-Air:[/Users/suman] gfortran -v
Using built-in specs.
COLLECT_GCC=gfortran
COLLECT_LTO_WRAPPER=/opt/homebrew/Cellar/gcc/13.2.0/bin/../libexec/gcc/aarch64-apple-darwin22/13/lto-wrapper
Target: aarch64-apple-darwin22
Configured with: ../configure --prefix=/opt/homebrew/opt/gcc --libdir=/opt/homebrew/opt/gcc/lib/gcc/current --disable-nls --enable-checking=release --with-gcc-major-version-only --enable-languages=c,c++,objc,obj-c++,fortran --program-suffix=-13 --with-gmp=/opt/homebrew/opt/gmp --with-mpfr=/opt/homebrew/opt/mpfr --with-mpc=/opt/homebrew/opt/libmpc --with-isl=/opt/homebrew/opt/isl --with-zstd=/opt/homebrew/opt/zstd --with-pkgversion='Homebrew GCC 13.2.0' --with-bugurl=https://github.com/Homebrew/homebrew-core/issues --with-system-zlib --build=aarch64-apple-darwin22 --with-sysroot=/Library/Developer/CommandLineTools/SDKs/MacOSX13.sdk\
Thread model: posix\
Supported LTO compression algorithms: zlib zstd\
gcc version 13.2.0 (Homebrew GCC 13.2.0)
3. NCL version 6.6.2 will be considered here.
4. Few software such as:\
  a) export CC=gcc FC=gfortran F77=gfortran f90=gfortran CXX=g++\
  b) zlib-1.2.11.tar (./configure --prefix=/Users/sumanmaity/allinstall; make; make install)\
  c) szip-2.1.1.tar (./configure --prefix=/Users/sumanmaity/allinstall; make; make install)\
  d) hdf5-1.10.7 (./configure --prefix=/Users/sumanmaity/allinstall --with-zlib=/Users/sumanmaity/allinstall --with-szlib=/Users/sumanmaity/allinstall --enable-fortran ; make; make           install). ***--enable-fortran: by default it is not enabled and therefore hdf5.mod file in the include will not be created and later on fortran code using HDF5 will not be              compiled. for creating hdf5.mod, use --enable-fortran\
 e) export CPPFLAGS="-I/Users/sumanmaity/allinstall/include" and export LDFLAGS="-L/Users/sumanmaity/allinstall/lib"
 f) netcdf-c-4.5.0  (./configure --prefix=/Users/sumanmaity/allinstall; make; make install)
 g) edit your .bashrc:
      export PATH=$PATH:/Users/sumanmaity/allinstall/bin in the .bashrc file
      source it.
 h) export CFLAGS="-Wno-implicit-function-declaration"
      udunits-2.1.2 (./configure --prefix=/Users/sumanmaity/allinstall/; make; make install)
 i) netcdf-fortran-4.5.1
      export CPPFLAGS="-I/Users/sumanmaity/allinstall/include"
      export LDFLAGS="-L/Users/sumanmaity/allinstall/lib"
      export CFLAGS="-Wno-implicit-function-declaration"
      export FFLAGS="-fallow-argument-mismatch"
      export FCFLAGS="-fallow-argument-mismatch"
      ./configure --prefix=/Users/sumanmaity/allinstall/; make; make install
6. proj is an important software for NCL installation.
   a) download proj-7.2.1.tar.gz and untar it
   b) cd proj-7.2.1
   c) Make the following corrections:
         vi src/proj_json_streaming_writer.hpp
         1. add #include <cstdint> after #include <vector> and #include <string>
         2. modify line 42 and 43 as below:
            typedef int64_t GIntBig;
            typedef uint64_t GUInt64;
         That's it.
   d) mkdir build && cd build
   e) cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local/proj7
   f) make -j4
   g) sudo make install
   h) export PATH=/usr/local/proj7/bin:$PATH and export DYLD_LIBRARY_PATH=/usr/local/proj7/lib:$DYLD_LIBRARY_PATH
8.
9.   
