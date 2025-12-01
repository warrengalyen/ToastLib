cmake_minimum_required(VERSION 3.10)
project(ToastLib C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# ToastLib library
add_library(toastlib STATIC
    libsrc/C_wrapper.c
    libsrc/callback.c
)

# Disable strict type checking for MinGW compatibility
if(MINGW)
    target_compile_options(toastlib PRIVATE -std=gnu99)
    # Define FORCEINLINE for GCC/MinGW
    target_compile_definitions(toastlib PRIVATE FORCEINLINE=inline)
endif()

# Set include directories for ToastLib
# The library source files include headers with paths relative to libsrc directory
target_include_directories(toastlib PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/libsrc
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# Public include directory
target_include_directories(toastlib PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# Link required Windows libraries
if(WIN32)
    target_link_libraries(toastlib
        ole32
        uuid
        oleaut32
        runtimeobject
    )
endif()

# Export the library
set(TOASTLIB_LIBRARY toastlib PARENT_SCOPE)
set(TOASTLIB_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include PARENT_SCOPE)

