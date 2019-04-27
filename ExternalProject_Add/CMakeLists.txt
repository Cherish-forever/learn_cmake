cmake_minimum_required (VERSION 3.10)

project(download_resources)

include(ExternalProject)

ExternalProject_Add(googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG releases/v1.8.1
  )

ExternalProject_Get_Property(googletest SOURCE_DIR)
ExternalProject_Get_Property(googletest STAMP_DIR)

add_custom_command(
  OUTPUT ${PROJECT_NAME}.download.googletest
  COMMENT "Download googletest done!"
  COMMAND cmake -E touch ${PROJECT_NAME}.download.googletest
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  DEPENDS googletest
  )

add_custom_target(${PROJECT_NAME} ALL
  COMMAND ls ${SOURCE_DIR}
  COMMENT "target done!"
  DEPENDS ${PROJECT_NAME}.download.googletest
  )

