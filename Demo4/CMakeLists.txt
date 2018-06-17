# cmake minimum version number
cmake_minimum_required (VERSION 2.8)

# project information
project (Demo4)

# add config files
configure_file (
  "${PROJECT_SOURCE_DIR}/config.h.in"
  "${PROJECT_BINARY_DIR}/config.h"
  )

option (
  USE_MYMATH
  "Use provided math implementation" ON
  )

if (USE_MYMATH)
  include_directories ("${PROJECT_SOURCE_DIR}/math")
  # add match dir
  add_subdirectory (math)
  set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (USE_MYMATH)
  

# specify exectuable file
# rename all source files to DIR_SRC
aux_source_directory (. DIR_SRCS)

add_executable (Demo ${DIR_SRCS})

# add link to library
target_link_libraries (Demo ${EXTRA_LIBS})
