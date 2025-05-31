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
  d) hdf5-1.10.7 (./configure --prefix=/Users/sumanmaity/allinstall --with-zlib=/Users/sumanmaity/allinstall --with-szlib=/Users/sumanmaity/allinstall --enable-fortran ; make; make           install).\
      --enable-fortran: by default it is not enabled and therefore hdf5.mod file in the include will not be created and later on fortran code using HDF5 will not be compiled. for             creating hdf5.mod, use --enable-fortran
 e) export CPPFLAGS="-I/Users/sumanmaity/allinstall/include" and export LDFLAGS="-L/Users/sumanmaity/allinstall/lib"\
 f) netcdf-c-4.5.0  (./configure --prefix=/Users/sumanmaity/allinstall; make; make install)\
 g) edit your .bashrc:
      export PATH=$PATH:/Users/sumanmaity/allinstall/bin in the .bashrc file and source it.\
 h) export CFLAGS="-Wno-implicit-function-declaration"\
      udunits-2.1.2 (./configure --prefix=/Users/sumanmaity/allinstall/; make; make install)
 i) netcdf-fortran-4.5.1
      export CPPFLAGS="-I/Users/sumanmaity/allinstall/include"\
      export LDFLAGS="-L/Users/sumanmaity/allinstall/lib"\
      export CFLAGS="-Wno-implicit-function-declaration"\
      export FFLAGS="-fallow-argument-mismatch"\
      export FCFLAGS="-fallow-argument-mismatch"\
      ./configure --prefix=/Users/sumanmaity/allinstall/; make; make install
5. proj is an important software for NCL installation.\
   a) download proj-7.2.1.tar.gz and untar it\
   b) cd proj-7.2.1\
   c) Make the following corrections:\
         vi src/proj_json_streaming_writer.hpp\
         c.1. add #include <cstdint> after #include <vector> and #include <string>\
         c.2. modify line 42 and 43 as below:\
            typedef int64_t GIntBig;\
            typedef uint64_t GUInt64;\
         That's it.\
   d) mkdir build && cd build\
   e) cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local/proj7\
   f) make -j4\
   g) sudo make install\
   h) export PATH=/usr/local/proj7/bin:$PATH and export DYLD_LIBRARY_PATH=/usr/local/proj7/lib:$DYLD_LIBRARY_PATH
6. Also install libpng-1.6.48.tar.gz and jasper-2.0.10.tar.gz similarly.
# Start installation
step 1: Download ncl_ncarg-6.6.2.tar.gz (https://www.earthsystemgrid.org/api/v1/dataset/ncl.662.src/file/ncl_ncarg-6.6.2.tar.gz) and untar it.\
step 2: cd ncl_ncarg-6.6.2/config\
    make -f Makefile.ini\
  ./ymake -config `pwd`\
  It will show “ymake: Unknown machine type” as the script doesn't recognise the system. You need to modify ymake to identify your system. Do the following:\
  As the system is “Darwin” and “arm64”, so put the following into ymake:\
        case    arm64:\
            set model   = $mach\
            set arch    = $mach\
            set sysincs = Darwin_arm64\
            set vendor  = Apple\
            breaksw\
  Add these lines in the ymake file at line 424-429. This “Darwin_arm64”, will be required to prepare the system file.\
  (modified "ymake" file is available in this repository. Download and put at ncl_ncarg-6.6.2/config/)\
step 3: create a file Darwin_arm64. Prepared "Darwin_arm64" is available in this repository. Just download it put at ncl_ncarg-6.6.2/config/.\
  then repeat step 2 and you will not see the "Unknown machine type" message. and the corresponding makefile will be created.
step 4: cd ../\
Now the code is ready to configure. during configuration step it will ask couple of queries and based on choice it will create/modify the makefile.\
$./Configure -v\
Build NCL (y)? Y\
Parent installation directory : /Users/suman/ncl_ncarg-6.6.2\
System temp space directory   : /tmp\
Build NetCDF4 feature support (optional)? (y): y\
Build HDF4 support (optional) into NCL? (y): n\
Also build HDF4 support (optional) into raster library? (y): n\
Build Triangle support (optional) into NCL (y): n\
If you are using NetCDF V4.x, did you enable NetCDF-4 support (y)?: y\
Did you build NetCDF with OPeNDAP support (y)?: n\
Build GDAL support (optional) into NCL? (n): n\
Build EEMD support (optional) into NCL? (y): n\
Build Udunits-2 support (optional) into NCL (y): y\
Build Vis5d+ support (optional) into NCL (n): n\
Build HDF-EOS2 support (optional) into NCL (y): n\
Build HDF5 support (optional) into NCL (y): y\
Build HDF-EOS5 support (optional) into NCL (y): n\
Build GRIB2 support (optional) into NCL (y): n\
Enter local library search path(s): /opt/X11/lib /Users/suman/allinstall/lib /opt/homebrew/lib\
Enter local include search path(s): /opt/X11/include /opt/X11/include/freetype2 /Users/suman/allinstall/include /opt/homebrew/include\
Go back and make more changes or review? (n): n\
Save current configuration ? (y): y

Next step is pretty simple: make Everything >& make-output & and tail -f make-output. Now if you do this without going further only 49 executables will be created and most importantly "ncl" will not be created. For a successfull installation, 59 executables need to be created. You need to do the following steps:
  1) vi ni/src/lib/nfpfort/yMakefile
     line no. 44 (meemd.o dpsort_large.o wrf_pw.o wrf_wind.o wrf_constants.o wrf_constants.mod) will be modified as:\
     meemd.o dpsort_large.o wrf_pw.o wrf_wind.o wrf_constants.o\
     (modified "yMakefile" is available in this repository as "ni_yMakefile", download and rename as "yMakefile" and put at ni/src/lib/nfpfort/)
  2) vi ni/src/lib/hlu/Format.c
    line no. 943 and 1286\
      int len1,len2,len3,len4; will be changed to long len1,len2,len3,len4;\
      (modified "Format.c" is available in this repository as "Format.c", download and put at ni/src/lib/hlu/)
  3) vi ncarg2d/src/libncarg_gks/bwi/argb2ci.f
     line 19-22 will be replaced as:\
        parameter (ARGBMASK = int(Z'40000000'))\
        parameter (RMASK     = int(Z'00FF0000'))\
        parameter (GMASK     = int(Z'0000FF00'))\
        parameter (BMASK     = int(Z'000000FF'))\
     and line 34-35 will be replaced as:\
       r = (iand(index, RMASK) / int(Z'0000FFFF')) / 255.\
       g = (iand(index, GMASK) / int(Z'000000FF')) / 255.\
     (modified "argb2ci.f" is available in this repository as "argb2ci.f", download and put at ncarg2d/src/libncarg_gks/bwi/)
  4) vi ncarg2d/src/libncarg/conpack/CodeIftran\
     line 9669 will be replaced as CBUF(1:lbuf)=' '\
     and line 9791 will be replaced as CBUF(1:LBUF)='0'\
     (modified "CodeIftran" is available in this repository as "CodeIftran", download and put at ncarg2d/src/libncarg/conpack/)
  5)  vi common/src/libncarg_c/yMakefile
     add the following:\
      a) after line 44\
        #elif defined(Darwin) && defined(arm64) && defined(__LP64__)\
        EXCSRCS = bcopyswap.c logic32.c\
        EXFSRCS = gbytes.f sbytes.f\
        EXOBJS  = sbytes.o gbytes.o bcopyswap.o logic32.o\
     b) after line 53\
        #elif defined(Darwin) && defined(arm64) && !defined(__LP64__)\
        EXCSRCS = bcopyswap.c logic32.c gsbytes.c\
        EXOBJS  = bcopyswap.o logic32.o gsbytes.o\
    (modified "yMakefile" is available in this repository as "common_yMakefile", download, rename and put at common/src/libncarg_c/)
  6) vi ncarview/src/bin/ictrans/yMakefile\
     after line 42\
     all-local::\
        @$(CAT) Copyright\
     modify as:\
     all-local::
  7) After all these above corrections and then:\
        make Everything >& make-output & and tail -f make-output\
     will install 57 executables and importantly, ncl is not installed. make-output will show the following error:\
     duplicate symbol '_theoptr' in:\
        NclApi.o\
        guiFuncs.o\
     duplicate symbol '_thefptr' in:\
        NclApi.o\
        guiFuncs.o\
     duplicate symbol '_cmd_line' in:\
        NclApi.o\
        guiFuncs.o\
     duplicate symbol '_possibleDimNames' in:\
        NclNewHDF5.o\
        NclHDF5.o\
     ld: 4 duplicate symbols for architecture arm64\
     collect2: error: ld returned 1 exit status\
     make[4]: *** [ncl] Error 1\
     Now few more files need to be modified.\
       a) vi ni/src/ncl/NclGlobalVars.h\
           int cmd_line = 0;\
           FILE *thefptr = NULL;\
           FILE *theoptr = NULL;\
     will be modified as:\
           extern int  cmd_line;\
           extern FILE *thefptr;\
           extern FILE *theoptr;\
     b) vi ni/src/ncl/NclApi.c\
           FILE *thefptr;\
           FILE *theoptr;\
           int cmd_line;\
	  Will be modified as:\
           FILE *thefptr = NULL\
           FILE *theoptr = NULL\
           int cmd_line = 0;\
     c) vi  ni/src/ncl/NclHDF5.c\
        At line 243:\
          NclQuark possibleDimNames[NUMPOSDIMNAMES];\
   Will be modified to\
          static NclQuark possibleDimNames[NUMPOSDIMNAMES];\
     d) vi  ni/src/ncl/NclNewHDF5.c\
      At line 85:\
        NclQuark possibleDimNames[NUMPOSDIMNAMES];\
   Will be modified to\
        static NclQuark possibleDimNames[NUMPOSDIMNAMES];\
The modifed NclGlobalVars.h, NclApi.c, NclHDF5.c, NclNewHDF5.c are available in the repository. Download and put at ni/src/ncl\
Now you are all done. Just make Everything >& make-output & and tail -f make-output\
It will take few minutes. After successfull completion, you will have a total of 59 executables including "ncl" too.\
Now modify and add the follwing lines to your .bashrc file:
export NCARG_ROOT=/Users/suman/ncl_ncarg-6.6.2
export PATH=$NCARG_ROOT/bin:$PATH
In addition .hluresfile file is provided in the repository as "hluresfile". Download and put as .hluresfile at your home directory\
That's it. Enjoy now.

References:
1. https://github.com/NCAR/ncl/compare/NCAR:8f9e947...MicroTed:c4486cc
2. https://www.ncl.ucar.edu/Download/build_from_src.shtml


     


     

     
     




