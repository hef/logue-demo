

if (UNIX AND NOT APPLE)
    set(arm-url https://developer.arm.com/-/media/Files/downloads/gnu-rm/5_4-2016q3/gcc-arm-none-eabi-5_4-2016q3-20160926-linux.tar.bz2)
    set(arm-url-hash SHA1=e87bbefaccbb43d30c084faeb945f4e96e7ec780)
elseif (APPLE)
    set(arm-url https://developer.arm.com/-/media/Files/downloads/gnu-rm/5_4-2016q3/gcc-arm-none-eabi-5_4-2016q3-20160926-mac.tar.bz2)
    set(arm-url-hash SHA1=defb787491a64d24f25b2e3c190170f735d4f14d)
elseif (WIN32)
    set(arm-url https://developer.arm.com/-/media/Files/downloads/gnu-rm/5_4-2016q3/gcc-arm-none-eabi-5_4-2016q3-20160926-win32-zip.zip)
    set(arm-url-hash SHA1=f6c05562b5f031bf79ec13c6f7311245c717be1b)
endif ()

FetchContent_Declare(
        arm-toolchain
        URL ${arm-url}
        URL_HASH ${arm-url-hash}
)

FetchContent_MakeAvailable(arm-toolchain)
set(CMAKE_SYSTEM_NAME logue PARENT_SCOPE)
set(CMAKE_SYSTEM_PROCESSOR arm PARENT_SCOPE)
set(CMAKE_C_COMPILER ${arm-toolchain_SOURCE_DIR}/bin/arm-none-eabi-gcc PARENT_SCOPE)
set(CMAKE_CXX_COMPILER ${arm-toolchain_SOURCE_DIR}/bin/arm-none-eabi-g++ PARENT_SCOPE)
set(CMAKE_SYSROOT ${arm-toolchain_SOURCE_DIR} PARENT_SCOPE)