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
  cmake -Bbuild -H. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./dist
  cmake --build build --target install 
```

Installs to `./dist/sqlite3/3.19.3.0/` 
