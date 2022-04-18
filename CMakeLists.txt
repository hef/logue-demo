cmake_minimum_required(VERSION 3.22)

include(FetchContent)

FetchContent_Declare(
        arm-toolchain
        URL https://developer.arm.com/-/media/Files/downloads/gnu-rm/5_4-2016q3/gcc-arm-none-eabi-5_4-2016q3-20160926-mac.tar.bz2
        URL_HASH  SHA1=defb787491a64d24f25b2e3c190170f735d4f14d
)
FetchContent_Declare(
        logue-sdk
        GIT_REPOSITORY https://github.com/korginc/logue-sdk.git
        GIT_TAG  ab654af0e29e5eb6eab7289d05699b45417d1f68
)
FetchContent_MakeAvailable(arm-toolchain)
FetchContent_MakeAvailable(logue-sdk)

set(LDDIR ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/demos/waves/ld)
set(RULESPATH ${LDDIR})
set(LDSCRIPT ${LDDIR}/userosc.ld)
set(LDOPT "-Xlinker --just-symbols=${LDDIR}/osc_api.syms")

set(CMAKE_SYSTEM_NAME logue)
set(CMAKE_SYSTEM_PROCESSOR arm)
SET(CMAKE_C_COMPILER ${arm-toolchain_SOURCE_DIR}/bin/arm-none-eabi-gcc)
SET(CMAKE_CXX_COMPILER ${arm-toolchain_SOURCE_DIR}/bin/arm-none-eabi-g++)
set(CMAKE_SYSROOT ${arm-toolchain_SOURCE_DIR})

SET(MCU cortex-m4)
set(MCFLAGS -mcpu=${MCU})
set(TOPT "-mthumb -mno-thumb-interwork -DTHUMB_NO_INTERWORKING -DTHUMB_PRESENT")
SET(OPT "-g -Os -mlittle-endian -mfloat-abi=hard -mfpu=fpv4-sp-d16 -fsingle-precision-constant -fcheck-new")
set(COPT "-std=c11 -mstructure-size-boundary=8")
set(CXXOPT "-std=c++11 -fno-rtti -fno-exceptions -fno-non-call-exceptions")
set(CWARN "-W -Wall -Wextra")
set(DDEFS "-DSTM32F401xC -DCORTEX_USE_FPU=TRUE -DARM_MATH_CM4 -D__FPU_PRESENT")
set(DEFS "${DDEFS}")


SET(CMAKE_C_FLAGS "${MCFLAGS} ${TOPT} ${OPT} ${COPT} ${CWARN} ${DEFS}" )
SET(CMAKE_CXX_FLAGS "${MCFLAGS} ${TOPT} ${OPT} ${CXXOPT} ${CXXWARN} ${DEFS}" )
set(CMAKE_EXE_LINKER_FLAGS "${MCFLAGS} ${TOPT} ${OPT} -nostartfiles -Wl,-Map=${CMAKE_CURRENT_BINARY_DIR}/${PROJECT}.map,--cref,--no-warn-mismatch,--library-path=${RULESPATH},--script=${LDSCRIPT} ${LDOPT}")




project(logue-demo)


add_executable(waves waves.cpp waves.hpp)
target_include_directories(waves PRIVATE ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/inc)
target_include_directories(waves PRIVATE ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/inc/dsp)
target_include_directories(waves PRIVATE ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/inc/utils)
target_include_directories(waves PRIVATE ${logue-sdk_SOURCE_DIR}/platform/ext/CMSIS/CMSIS/Include)


