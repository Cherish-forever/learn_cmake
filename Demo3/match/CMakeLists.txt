# specify exectuable file
# rename all source files to DIR_SRC
aux_source_directory (. DIR_LIB_SRCS)
add_library (MatchFunction ${DIR_LIB_SRCS})
