set(LIB_NAME "common-freertos-posix")
set(LIB_ALIAS "Common::FreeRTOS::Platform")



add_library(${LIB_NAME} INTERFACE)
add_library(${LIB_ALIAS} ALIAS ${LIB_NAME})

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)
find_package(FreeRTOS COMPONENTS POSIX REQUIRED)

target_link_libraries(${LIB_NAME}
    INTERFACE
        Threads::Threads
        FreeRTOS::POSIX
)
