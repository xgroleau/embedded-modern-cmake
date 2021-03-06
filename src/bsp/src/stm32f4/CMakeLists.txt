set(LIB_NAME "common-bsp-stm32")
set(LIB_ALIAS "Common::BSP::Platform")

set(LIB_SOURCES
    bsp.cpp
)

set(LIB_PRIVATE_HEADERS
    stm32f4xx_hal_conf.h
)


stm32_fetch_cmsis(F4)
stm32_fetch_hal(F4)
stm32_fetch_cube(F4)

find_package(CMSIS COMPONENTS
            STM32F429ZI
            REQUIRED)

find_package(HAL COMPONENTS 
            STM32F429ZI
            REQUIRED)


add_library(${LIB_NAME} ${LIB_SOURCES} ${LIB_PRIVATE_HEADERS})
add_library(${LIB_ALIAS} ALIAS ${LIB_NAME})

target_include_directories(${LIB_NAME} 
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        ${BSP_CMAKE_DIR}/include
)


target_link_libraries(${LIB_NAME}
PUBLIC
        # Defined libraries
        Common::FreeRTOS

        # Need to be public for the linker script
        CMSIS::STM32::F429ZI

PRIVATE
        #stm32
        STM32::NoSys

        HAL::STM32::F4::RCC
        HAL::STM32::F4::GPIO
        HAL::STM32::F4::CORTEX

)


add_custom_target(
    flash
    COMMAND openocd  -f ${OPENOCD_CFG_PATH} -c 'program $<TARGET_FILE:${EXECUTABLE_NAME}> verify exit'
)
