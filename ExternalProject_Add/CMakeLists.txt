cmake_minimum_required (VERSION 3.10)

PROJECT(download_resources C ASM)
include(arm-none-eabi-gcc-toolchain.cmake)
include(ExternalProject)

set(external_project_name "stm32f4xx")
ExternalProject_Add(${external_project_name}
  URL ${CMAKE_CURRENT_SOURCE_DIR}/en.stm32cubef4.zip
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND ""
  CMAKE_COMMAND ""
  BUILD_COMMAND ""
  INSTALL_COMMAND ""
  )

ExternalProject_Get_Property(${external_project_name} SOURCE_DIR)
ExternalProject_Get_Property(${external_project_name} STAMP_DIR)

add_custom_command(
  OUTPUT ${PROJECT_NAME}.download.done.stamp
  COMMENT "${external_project_name} download done"
  COMMAND cmake -E touch ${PROJECT_NAME}.download.done.stamp
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  DEPENDS ${external_project_name})

set(${PROJECT_NAME}_COMMON_COMPILE_CFLAG " -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -DSTM32 -DSTM32F4 -DUSE_HAL_DRIVER -g3 -Wall -fmessage-length=0 -ffunction-sections -std=c11")
set(${PROJECT_NAME}_HAL_SOURCE_DIR  ${SOURCE_DIR}/Drivers/STM32F4xx_HAL_Driver/Src)
set(${PROJECT_NAME}_HAL_INCLUDE_DIR ${SOURCE_DIR}/Drivers/STM32F4xx_HAL_Driver/Inc)
set(${PROJECT_NAME}_CMSIS_INCLUDE_DIR ${SOURCE_DIR}/Drivers/CMSIS/Include)
set(${PROJECT_NAME}_CMSIS_DEVICE_INCLUDE_DIR ${SOURCE_DIR}/Drivers/CMSIS/Device/ST/STM32F4xx/Include)
list(APPEND STM32F4_HAL_INC
  ${${PROJECT_NAME}_HAL_INCLUDE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${${PROJECT_NAME}_CMSIS_INCLUDE_DIR}
  ${${PROJECT_NAME}_CMSIS_DEVICE_INCLUDE_DIR}
  )
list(APPEND STM32F4_HAL_FILES
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_adc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_adc_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_can.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_cec.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_cortex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_crc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_cryp.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_cryp_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dac.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dac_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dcmi.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dcmi_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dfsdm.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dma.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dma2d.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dma_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_dsi.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_eth.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_flash.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_flash_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_flash_ramfunc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_fmpi2c.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_fmpi2c_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_gpio.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_hash.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_hash_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_hcd.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_i2c.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_i2c_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_i2s.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_i2s_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_irda.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_iwdg.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_lptim.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_ltdc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_ltdc_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_mmc.c
#  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_msp_template.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_nand.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_nor.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_pccard.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_pcd.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_pcd_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_pwr.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_pwr_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_qspi.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_rcc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_rcc_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_rng.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_rtc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_rtc_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_sai.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_sai_ex.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_sd.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_sdram.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_smartcard.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_spdifrx.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_spi.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_sram.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_tim.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_tim_ex.c
#  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_timebase_rtc_alarm_template.c
#  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_timebase_rtc_wakeup_template.c
#  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_timebase_tim_template.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_uart.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_usart.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_hal_wwdg.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_adc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_crc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_dac.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_dma.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_dma2d.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_exti.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_fmc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_fsmc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_gpio.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_i2c.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_lptim.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_pwr.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_rcc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_rng.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_rtc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_sdmmc.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_spi.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_tim.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_usart.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_usb.c
  ${${PROJECT_NAME}_HAL_SOURCE_DIR}/stm32f4xx_ll_utils.c
  )

#add_compile_options("${${PROJECT_NAME}_COMMON_COMPILE_CFLAG}")
set(STM32F4xx_HAL_LIST "STM32F446xx;STM32F405xx;STM32F401xC")
foreach(STM32F4xx_HAL ${STM32F4xx_HAL_LIST})
#  set(${STM32F4xx_HAL}_HAL_CFLAGS "${STM32F4xx_HAL}_HAL_CFLAGS ${${PROJECT_NAME}_COMMON_COMPILE_CFLAG}")
#  set(${STM32F4xx_HAL}_HAL_CFLAGS "${STM32F4xx_HAL}_HAL_CFLAGS -D${STM32F4xx_HAL}")
  add_library(${STM32F4xx_HAL} STATIC)
  target_sources(${STM32F4xx_HAL} PRIVATE ${STM32F4_HAL_FILES})
  target_include_directories(${STM32F4xx_HAL} PRIVATE ${STM32F4_HAL_INC})
  set_target_properties(${STM32F4xx_HAL} PROPERTIES COMPILE_FLAGS ${${PROJECT_NAME}_COMMON_COMPILE_CFLAG})
  target_compile_definitions(${STM32F4xx_HAL}
    PRIVATE "-D${STM32F4xx_HAL}")
add_dependencies(${STM32F4xx_HAL} ${external_project_name})
endforeach()

add_custom_target(${PROJECT_NAME} ALL
  COMMAND ls ${SOURCE_DIR}
  COMMENT "target done!"
  DEPENDS ${PROJECT_NAME}.download.done.stamp
  )
