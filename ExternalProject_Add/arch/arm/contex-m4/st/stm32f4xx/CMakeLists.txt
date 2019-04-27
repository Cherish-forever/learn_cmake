cmake_minimum_required (VERSION 3.10)

project(stm32f4xx C ASM)

set(CMAKE_C_COMPILER   arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)

set(${PROJECT_NAME}_COMMON_COMPILE_CFLAG "-mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -DSTM32 -DSTM32F4 -DUSE_HAL_DRIVER -g3 -Wall -fmessage-length=0 -ffunction-sections -std=c11")
set(${PROJECT_NAME}_HAL_SOURCE_DIR           ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/STM32F4xx_HAL_Driver/Src)
set(${PROJECT_NAME}_HAL_INCLUDE_DIR          ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/STM32F4xx_HAL_Driver/Inc)
set(${PROJECT_NAME}_CMSIS_INCLUDE_DIR        ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/CMSIS/Include)
set(${PROJECT_NAME}_CMSIS_DEVICE_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/CMSIS/Device/ST/STM32F4xx/Include)

list(APPEND STM32F4_HAL_INC
  ${${PROJECT_NAME}_HAL_INCLUDE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${${PROJECT_NAME}_CMSIS_INCLUDE_DIR}
  ${${PROJECT_NAME}_CMSIS_DEVICE_INCLUDE_DIR}
  )

file(GLOB STM32F4xx_HAL_FILES
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/*.c
  )

message("hal file is ${STM32F4xx_HAL_FILES}")

#string(REGEX REPLACE "${${PROJECT_NAME}_HAL_SOURCE_DIR}/*template.c;"
#  "" STM32F4xx_HAL_FILES
#  ${STM32F4xx_HAL_ALL_FILES})

add_custom_target(${PROJECT_NAME} ALL
  COMMAND cmake -E copy ${${PROJECT_NAME}_HAL_INCLUDE_DIR}/stm32f4xx_hal_conf_template.h ${${PROJECT_NAME}_HAL_INCLUDE_DIR}/stm32f4xx_hal_conf.h
  COMMAND cmake -E remove -f  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx*_template.c
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  )

set(STM32F4xx_HAL_LIST "STM32F446xx;STM32F405xx;STM32F401xC")
foreach(STM32F4xx_HAL ${STM32F4xx_HAL_LIST})
  add_library(${STM32F4xx_HAL} STATIC)
  target_sources(${STM32F4xx_HAL} PRIVATE ${STM32F4xx_HAL_FILES})
  target_include_directories(${STM32F4xx_HAL} PRIVATE ${STM32F4_HAL_INC})
  set_target_properties(${STM32F4xx_HAL} PROPERTIES COMPILE_FLAGS ${${PROJECT_NAME}_COMMON_COMPILE_CFLAG})
  target_compile_definitions(${STM32F4xx_HAL}
    PRIVATE "-D${STM32F4xx_HAL}")
  add_dependencies(${STM32F4xx_HAL} ${PROJECT_NAME})
endforeach()
