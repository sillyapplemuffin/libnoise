# libnoise-cmake

A CMake wrapper for [libnoise](http://libnoise.sourceforge.net/index.html) v1.0.0, a C++ library for generating
coherent noise.

##             

This repository provides a CMake build system for `libnoise`, making it easy to integrate using `FetchContent`. It also
includes the `noiseutils` library and examples from the original project. Mostly for me to understand what I was doing,
which I probably didn't, so use it at your own risk.

## Building

To build the project, you'll need CMake 3.15 or higher.

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

### Options

The following CMake options are available:

* `BUILD_SHARED_LIBS`: Build shared libraries (ON by default).
* `BUILD_LIBNOISE_UTILS`: Build the `noiseutils` library (ON by default).
* `BUILD_LIBNOISE_EXAMPLES`: Build the examples (ON by default).
* `BUILD_LIBNOISE_DOCUMENTATION`: Create Doxygen documentation (OFF by default).

## Usage with FetchContent

To use `libnoise` in your project, you can use `FetchContent`:

```cmake
include(FetchContent)

FetchContent_Declare(
        libnoise
        GIT_REPOSITORY https://github.com/sillyapplemuffin/libnoise.git
        GIT_TAG main
)

FetchContent_MakeAvailable(libnoise)

# Link against the libraries
target_link_libraries(your_target PRIVATE noise::noise noiseutils::noiseutils)
```

## Credits

This project is a wrapper around the original `libnoise` library. All credit for the library itself goes to the original
authors.

## License

The `libnoise` library is licensed under the GNU Lesser General Public License (LGPL). See the `LICENSE.md` file for
more details.
