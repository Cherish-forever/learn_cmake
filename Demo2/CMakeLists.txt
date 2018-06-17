# cmake minimum version number
cmake_minimum_required (VERSION 2.8)

# project information
project (Demo)

# specify exectuable file, for type 1
#add_executable (Demo main.cc MatchFunction.cc)

# specify exectuable file, for type 2
# rename all source files to DIR_SRC
aux_source_directory (. DIR_SRCS)
add_executable (Demo ${DIR_SRCS})
