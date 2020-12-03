# Embedded modern cmake 

This project is used to develop modern C/C++ applications on embedded targets with gcc. 
The stm32f429zi is used as an example for this template.

## Building

If you have clang-tidy installed, the build will use it for static analysis.
Check the cmake options for more information.

### Host
Note that host compilation is supported, so you can run and test your application natively on your computer. Just need to mock the bsp.

```
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Debug ..
make
```

### Target
If you want to build for the embedded target, use the toolchain on your cmake build.
Note that you need arm-gcc-none-eabi available in your path, you can find it [here](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)

```
cmake -D CMAKE_BUILD_TYPE=Debug -D CMAKE_TOOLCHAIN_FILE=../cmake/stm32_f429zi_gcc.cmake .. 
```

Note that as of now, the tests can only be built on the native target.

#### Adding targets

To add a target, the only things needed are to add the target toolchain and add support for it's bsp and freertos.

## Flashing

Install [OpenOCD](http://openocd.org/). Then you can flash using the provided `make flash`command or directly via openocd.

```
openocd -f ./tools/openocd/stm32_f4/stm32_f4.cfg -c "program build/src/embedded-modern-cmake.elf verify reset exit"
```

## Tools

Some tools are integrated in the build. 

### Error checking

All errors from external libraries (gtests and freertos) are ignored. Lots of warnings are enabled and warnings are treated as errors. Modify the cmake options as needed.
Clang-tidy is used as a supplement for additional checks.

### Running tests
You can use `make test` or `ctest` to launch the tests.

### Formatting
You can run `make format` and `make check-format` to match the formatting convention used.

### Debugging
OpenOCD has a gdb server that defaults to port 3333, you can then connect to it using arm-none-eabi-gdb. The server can be launched using this command

```
openocd -f ./tools/openocd/stm32_f4/stm32_f4.cfg -c init -c \"reset init\"
```

### Doxygen
Doxygen documentation is generated on build if doxygen is found on path. A target `make doc` is available to only build the documentation

### CI

The CI runs verify the compilation on host and target. The formatting is checked and tests are run.
[LGTM](https://lgtm.com/) is used for addtional security/bug checks.

