# NCL_install_mac_arm64
This repository guides you how to install NCAR Command Language (NCL) using the source code
NCL (NCAR Command Language) was a very useful tool in the past. Many researchers who began their careers using GrADS gradually shifted to NCL/NCAR Graphics due to its powerful graphical capabilities, user-friendly interface, strong user community, and a wide range of readily available functions. Although NCL is no longer actively maintained, it remains a valuable tool for the atmospheric modeling community, both at the global and regional levels.\
In the past, installing NCL was extremely easy, thanks to the availability of precompiled binaries for various operating systems and compilers—installation typically required nothing more than extracting the archive. With the evolution of macOS, the last few versions of NCL included precompiled binaries compatible with macOS on Intel-based systems, which worked well.\
However, with the advent of Mac systems using M1/M2 (Apple Silicon) and the arm64 architecture, these older precompiled binaries no longer function. As a result, the only way to run NCL on Mac-arm64 systems is to build it from the source—a process that can be quite challenging. This document provides a step-by-step guide to installing NCL on Mac-arm64 systems.
# Prerequisites
Few points to be noted before start of the installation:
1. In this installation guide, NCL version 6.6.2 will be considered and installation process will be MacOS Sonoma version 14.3.1 will be used:\
suman@Sumans-Air:[/Users/suman] uname -a
Darwin Sumans-Air.lan 23.3.0 Darwin Kernel Version 23.3.0: Wed Dec 20 21:30:27 PST 2023; root:xnu-10002.81.5~7/RELEASE_ARM64_T8103 arm64 arm Darwin
2. Following version of gcc/gfortran will be used:
suman@Sumans-Air:[/Users/suman] gfortran -v
Using built-in specs.
COLLECT_GCC=gfortran
COLLECT_LTO_WRAPPER=/opt/homebrew/Cellar/gcc/13.2.0/bin/../libexec/gcc/aarch64-apple-darwin22/13/lto-wrapper
Target: aarch64-apple-darwin22
Configured with: ../configure --prefix=/opt/homebrew/opt/gcc --libdir=/opt/homebrew/opt/gcc/lib/gcc/current --disable-nls --enable-checking=release --with-gcc-major-version-only --enable-languages=c,c++,objc,obj-c++,fortran --program-suffix=-13 --with-gmp=/opt/homebrew/opt/gmp --with-mpfr=/opt/homebrew/opt/mpfr --with-mpc=/opt/homebrew/opt/libmpc --with-isl=/opt/homebrew/opt/isl --with-zstd=/opt/homebrew/opt/zstd --with-pkgversion='Homebrew GCC 13.2.0' --with-bugurl=https://github.com/Homebrew/homebrew-core/issues --with-system-zlib --build=aarch64-apple-darwin22 --with-sysroot=/Library/Developer/CommandLineTools/SDKs/MacOSX13.sdk
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 13.2.0 (Homebrew GCC 13.2.0)
3. 
   
4. 
