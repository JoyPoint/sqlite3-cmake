# sqlite3-cmake

[SQLite3](https://sqlite.org/download.html) built using CMake.

## Getting Started

Download and install [CMake](https://cmake.org/download/).

This requires at least CMake 3.8+ purely as a learning exercise in modern CMake
and to educate other CMake authors to keep up to date.

Centos and Ubuntu package managers only ship CMake 3.6 so you will want to 
download and install from the website to ensure you have the actual latest CMake.


## Windows

On Windows from Visual Studio command prompt

```
  cmake -Bbuild -H.  -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./distrib
  cmake --build build --target install 
```

## Linux / OSX
```
  cmake -Bbuild -H. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./distrib
  cmake --build build --target install 
``` 
