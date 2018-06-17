# cmake minimum version number
cmake_minimum_required (VERSION 2.8)

# project information
project (Demo3)

# specify exectuable file
# rename all source files to DIR_SRC
aux_source_directory (. DIR_SRCS)

# add match dir
add_subdirectory (match)

add_executable (Demo ${DIR_SRCS})

# add link to library
target_link_libraries (Demo MatchFunction)
