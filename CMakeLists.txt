cmake_minimum_required(VERSION 3.8)
project(sqlite3 
  VERSION 3.19.3.0
  LANGUAGES C
)

#############
# LIB sqlite3
#############
add_library(sqlite3 STATIC 
  src/sqlite3.c 
  src/sqlite3.h 
  src/sqlite3ext.h)
target_include_directories(sqlite3 
  PRIVATE
    ${CMAKE_SOURCE_DIR}/src
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
set_target_properties(sqlite PROPERTIES OUTPUT_NAME sqlite3)
target_compile_definitions(sqlite
  PRIVATE
  -DSQLITE_ENABLE_RTREE
  -DSQLITE_ENABLE_FTS4
  -DSQLITE_ENABLE_FTS5
  -DSQLITE_ENABLE_JSON1
  -DSQLITE_ENABLE_UNLOCK_NOTIFY
)

#########
# INSTALL 
#########
install(
  FILES 
    src/sqlite3.h 
    src/sqlite3ext.h 
  DESTINATION 
    ${PROJECT_NAME}/${PROJECT_VERSION}/include
)

install(TARGETS sqlite3 
  LIBRARY DESTINATION ${PROJECT_NAME}/${PROJECT_VERSION}/lib 
  ARCHIVE DESTINATION ${PROJECT_NAME}/${PROJECT_VERSION}/lib
)

install(TARGETS sqlite 
  RUNTIME DESTINATION ${PROJECT_NAME}/${PROJECT_VERSION}/bin
)
