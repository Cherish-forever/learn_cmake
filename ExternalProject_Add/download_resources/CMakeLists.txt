cmake_minimum_required (VERSION 3.10)

project(download_resources C ASM)
include(ExternalProject)

set(external_project_name "stm32f4xx")
ExternalProject_Add(${external_project_name}
  URL ${CMAKE_CURRENT_SOURCE_DIR}/en.stm32cubef4.zip
  UPDATE_COMMAND cp ${CMAKE_CURRENT_SOURCE_DIR}/../arch/arm/contex-m4/st/stm32f4xx/CMakeLists.txt ./
  INSTALL_COMMAND ""
  )

ExternalProject_Get_Property(${external_project_name} SOURCE_DIR)
ExternalProject_Get_Property(${external_project_name} STAMP_DIR)
