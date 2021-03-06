cmake_minimum_required(VERSION 2.8.4)

project(config)

#set(CMAKE_VERBOSE_MAKEFILE ON)

find_package(BISON REQUIRED)
find_package(FLEX REQUIRED)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

file(MAKE_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../include/generated")

BISON_TARGET(zconf zconf.y zconf.tab.c COMPILE_FLAGS "-l -b zconf -p zconf -t")
FLEX_TARGET(zconfscan zconf.l zconf.lex.c COMPILE_FLAGS "-Pzconf -L")

SET(zconf_deps ${FLEX_zconfscan_OUTPUTS})
SET_SOURCE_FILES_PROPERTIES(${BISON_zconf_OUTPUTS}
      PROPERTIES OBJECT_DEPENDS "${zconf_deps}")

set(conf_src conf.c ${BISON_zconf_OUTPUTS})

add_executable(conf ${conf_src})
