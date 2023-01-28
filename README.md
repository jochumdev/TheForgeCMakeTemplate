# Template for project's using The Forge with CMake

This is a basic template for compiling [The Forge](https://github.com/ConfettiFX/The-Forge) with [CMake](https://cmake.org/).

## Usage (presets)

Build with all examples (the download of "Art.zip" might take ages):

```bash
cmake --preset debug-clang-examples && cmake --build --preset debug-clang-examples
```

Build no examples:

```bash
cmake --preset debug-clang && cmake --build --preset debug-clang
```

## Usage (no presets)

Build with all examples (the download of "Art.zip" might take ages):

```bash
mkdir build; cd build
CC=/usr/bin/clang CXX=/usr/bin/clang++ cmake -DFORGE_BUILD_EXAMPLES=ON -G Ninja ..; cmake --build .
```

Build no examples:

```bash
mkdir build; cd build
CC=/usr/bin/clang CXX=/usr/bin/clang++ cmake -G Ninja ..; cmake --build .
```

## Authors

- [René Jochum](https://github.com/jochumdev)

## License

This Template is MIT Licensed.