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

Installs to `./dist/${CMAKE_SYSTEM_NAME}/${CMAKE_BUILD_TYPE}/${PROJECT_NAME}/${PROJECT_VERSION}/`

eg, 
```
./dist/Darwin/Release/sqlite3/3.19.3.0
  |-- include
  |-- lib
  \-- bin

./dist/share/cmake/sqlite3
  \-- sqlite3-config.cmake
```

## Packaging

```
  cmake --build build --target package
```

Output builds to `build/dist`.

Note that the default packaging is a zip for this project and the `install`
target is a dependency of `package`. So you get the same structure from `install`
within the resulting zip.

