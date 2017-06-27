cmake_minimum_required(VERSION 3.8)
project(sqlite3 
  VERSION 3.19.3.0
  LANGUAGES C
)

#############
# LIB sqlite3
#############

add_library(sqlite3
  src/sqlite3.c 
  src/sqlite3.h 
  src/sqlite3ext.h
)
target_include_directories(sqlite3 
  PRIVATE
    ${CMAKE_SOURCE_DIR}/src
)
set_target_properties(sqlite3
  PROPERTIES 
    VERSION ${PROJECT_VERSION}
)

target_compile_definitions(sqlite3
  PRIVATE
  -DSQLITE_ENABLE_RTREE
  -DSQLITE_ENABLE_FTS4
  -DSQLITE_ENABLE_FTS5
  -DSQLITE_ENABLE_JSON1
  -DSQLITE_ENABLE_UNLOCK_NOTIFY
)

#############
# BIN sqlite3
#############
option(BUILD_LIB_ONLY "Only build the library" OFF)

if(NOT BUILD_LIB_ONLY)
  add_executable(sqlite 
    src/sqlite3.c 
    src/shell.c 
    src/sqlite3.h 
    src/sqlite3ext.h
  )
  target_include_directories(sqlite 
    PRIVATE
      ${CMAKE_SOURCE_DIR}/src
  )
  
  set_target_properties(sqlite 
    PROPERTIES 
      OUTPUT_NAME sqlite3
  )
  target_compile_definitions(sqlite
    PRIVATE
    -DSQLITE_ENABLE_RTREE
    -DSQLITE_ENABLE_FTS4
    -DSQLITE_ENABLE_FTS5
    -DSQLITE_ENABLE_JSON1
    -DSQLITE_ENABLE_UNLOCK_NOTIFY
  )
endif(NOT BUILD_LIB_ONLY)

#########
# INSTALL 
#########

# Installation sub path to avoid overriding different versions and 
# release types when installing
set(INSTALL_SUBDIRS "${CMAKE_SYSTEM_NAME}/")

# Generate Version File
configure_file(
  "${CMAKE_SOURCE_DIR}/cmake/library-config-version.cmake.in"
  "${CMAKE_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake" @ONLY
)
### Install Headers
install(
  FILES 
    src/sqlite3.h 
    src/sqlite3ext.h 
  DESTINATION 
  ${INSTALL_SUBDIRS}include/${PROJECT_NAME}
)

### Install Library and add to CMake Export Config
install(TARGETS sqlite3 
  EXPORT sqlite3-config 
  LIBRARY DESTINATION ${INSTALL_SUBDIRS}lib/${PROJECT_NAME}
  ARCHIVE DESTINATION ${INSTALL_SUBDIRS}lib/${PROJECT_NAME}
  INCLUDES DESTINATION ${INSTALL_SUBDIRS}include/${PROJECT_NAME}
)

### Install Binary
if(NOT BUILD_LIB_ONLY)
  install(TARGETS sqlite 
    RUNTIME DESTINATION ${INSTALL_SUBDIRS}bin/${PROJECT_NAME}
  )
endif(NOT BUILD_LIB_ONLY)

### Generate Export Config sqlite3-config.cmake

install(EXPORT sqlite3-config 
  NAMESPACE SQLITE3::
  DESTINATION ${INSTALL_SUBDIRS}cmake
)
install(
  FILES 
  ${CMAKE_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake 
  DESTINATION 
  ${INSTALL_SUBDIRS}cmake
)


#########
# PACKAGE
#########
include (InstallRequiredSystemLibraries)

set(CPACK_GENERATOR "ZIP;TGZ")
set(CPACK_PACKAGE_VENDOR "SQLite")
# set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "")
# set(CPACK_PACKAGE_DESCRIPTION_FILE "")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")

set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
set(CPACK_PACKAGE_INSTALL_DIRECTORY "${PROJECT_NAME}-${PROJECT_VERSION}")

IF(WIN32 AND NOT UNIX)
  # There is a bug in NSI that does not handle full unix paths properly. Make
  # sure there is at least one set of four (4) backlasshes.
  # set(CPACK_PACKAGE_ICON "${CMake_SOURCE_DIR}/Utilities/Release\\\\InstallIcon.bmp")
  set(CPACK_NSIS_INSTALLED_ICON_NAME "bin\\\\${PROJECT_NAME}.exe")
  set(CPACK_NSIS_DISPLAY_NAME "${CPACK_PACKAGE_INSTALL_DIRECTORY}")
  set(CPACK_NSIS_HELP_LINK "http:\\\\\\\\www.sqlite.org")
  set(CPACK_NSIS_URL_INFO_ABOUT "http:\\\\\\\\www.sqlite.org/about.html")
  set(CPACK_NSIS_CONTACT "")
  set(CPACK_NSIS_MODIFY_PATH ON)
ELSE(WIN32 AND NOT UNIX)
  set(CPACK_STRIP_FILES "bin/${PROJECT_NAME}")
  set(CPACK_SOURCE_STRIP_FILES "")
ENDIF(WIN32 AND NOT UNIX)
set(CPACK_PACKAGE_EXECUTABLES sqlite ${PROJECT_NAME})


# Packages build to:
# http://stackoverflow.com/a/16942262/622276
set(CPACK_OUTPUT_FILE_PREFIX dist)

include(CPack)
