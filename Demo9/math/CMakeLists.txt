# specify exectuable file
# rename all source files to DIR_SRC
aux_source_directory (. DIR_LIB_SRCS)
add_library (MathFunctions ${DIR_LIB_SRCS})
target_compile_definitions(
  MathFunctions
  PRIVATE -DPRINTF_RESULT=0
  )

add_library(MathFunctions_Printf ${DIR_LIB_SRCS})
target_compile_definitions(
  MathFunctions_Printf
  PRIVATE -DPRINTF_RESULT=1
  )
