# cmake minimum version number
cmake_minimum_required (VERSION 2.8)

# project information
project (Demo8)

# add version
set (Demo_VERSION_MAJOR 1)
set (Demo_VERSION_MINOR 0)
set (Demo_VERSION_PATCH 0)

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
  set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions_Printf)
endif (USE_MYMATH)

# specify exectuable file
# rename all source files to DIR_SRC
aux_source_directory (. DIR_SRCS)

add_executable (Demo ${DIR_SRCS})

# add link to library
target_link_libraries (Demo ${EXTRA_LIBS})

install (TARGETS Demo DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/config.h"
  DESTINATION include)

# CPack, make a package
include (InstallRequiredSystemLibraries)
set (CPACK_RESOURCE_FILE_LICENSE
  "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
set (CPACK_PACKAGE_VERSION_MAJOR "${Demo_VERSION_MAJOR}")
set (CPACK_PACKAGE_VERSION_MINOR "${Demo_VERSION_MINOR}")
set (CPACK_PACKAGE_VERSION_PATCH "${Demo_VERSION_PATCH}")
include (CPack)
