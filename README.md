# sqlite3-cmake

[SQLite3](https://sqlite.org/download.html) built using CMake.

## Getting Started

Download and install [CMake](https://cmake.org/download/).

This requires at least CMake 3.8+ purely as a learning exercise in modern CMake
and to educate other CMake authors to keep up to date.

Centos and Ubuntu package managers only ship CMake 3.6 so you will want to 
download and install from the website to ensure you have the actual latest CMake.


## Getting Started

```
  git clone https://github.com/dexata/sqlite3-cmake.git sqlite3-cmake
  cd sqlite3-cmake

  # Defaults to building static library
  cmake -Bbuild -H. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./dist
  cmake --build build --target install 

  # Reconfigure for shared library
  cmake -Bbuild -H. -DBUILD_SHARED_LIBS=ON
  cmake --build build --target install 

  # Reconfigure to skip building the binary
  cmake -Bbuild -H. -DBUILD_LIB_ONLY=ON
  cmake --build build --target install 

```

Installs to `./dist/${CMAKE_SYSTEM_NAME}/`

eg, 
```
./dist/Darwin/
  |-- include/sqlite3/*.h
  |-- lib/sqlite3/*.a
  |-- bin
  |-- cmake/sqlite3-config.cmake
  \-- cmake/sqlite3-config-version.cmake

```

This is so that for the different permutations of Platform, build type and 
version can be installed and not collide.

The exported `sqlite3-config.cmake` bakes in the paths for that version.

## Windows

If you haven't discovered [Chocolatey](https://chocolatey.org/), a package 
manager for windows, you should go get it now.

Then from a command prompt with Administrative rights:
```
choco install cmake --installargs 'ADD_CMAKE_TO_PATH=""System""' -y
choco install ninja -y

:: Reload PATH variable
refreshenv
```

The benefits of CMake using a cache to point to it's defined compilers and
build system mean I can do things like the following. I have multiple build 
pipelines available side by side.

```
:: You may need to init Visual Studio variables
:: VS2013
"C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat" x64
cmake -H. -Bbuild-vs2013 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./dist/vs2013
```

Then in a new command prompt:

```
:: VS2017
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat" x64
cmake -H. -Bbuild-vs2017 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./dist/vs2017
```

Also in a new command prompt:

```
:: VS2017
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat" x64
cmake -H. -Bbuild-vs2017-ninja -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./dist/vs2017-ninja
```

Then I can run the following:

```
cmake --build build-vs2013 --target install
cmake --build build-vs2017 --target install
cmake --build build-vs2017-ninja --target install
```

If you want to get build times:

```
cmake -E time cmake --build build-vs2013 --target install
cmake -E time cmake --build build-vs2017 --target install
cmake -E time cmake --build build-vs2017-ninja --target install
```


## Packaging

```
  cmake --build build --target package
```

Output builds to `build/dist`.

Note that the default packaging is a zip for this project and the `install`
target is a dependency of `package`. So you get the same structure from `install`
within the resulting zip.

