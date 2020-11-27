set(LIB_NAME "common-bsp-posix")
set(LIB_ALIAS "Common::BSP::Platform")

set(LIB_SOURCES
    bsp.cpp
)

set(LIB_PRIVATE_HEADERS

)

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
)

add_custom_target(
    run
    COMMAND $<TARGET_FILE:${EXECUTABLE_NAME}>
)
