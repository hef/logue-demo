

set(LDDIR ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/demos/waves/ld)
set(RULESPATH ${LDDIR})
set(LDSCRIPT ${LDDIR}/userosc.ld)
set(LDOPT -Xlinker --just-symbols=${LDDIR}/osc_api.syms)



SET(MCU cortex-m4)
set(MCFLAGS "-mcpu=${MCU}")
set(TOPT -mthumb -mno-thumb-interwork -DTHUMB_NO_INTERWORKING -DTHUMB_PRESENT)
SET(OPT "-g -Os -mlittle-endian -mfloat-abi=hard -mfpu=fpv4-sp-d16 -fsingle-precision-constant -fcheck-new")
set(COPT "-std=c11 -mstructure-size-boundary=8")
set(CXXOPT "-std=c++11 -fno-rtti -fno-exceptions -fno-non-call-exceptions")
set(CWARN "-W -Wall -Wextra")
set(DDEFS "-DSTM32F401xC -DCORTEX_USE_FPU=TRUE -DARM_MATH_CM4 -D__FPU_PRESENT")
set(DEFS "${DDEFS}")

#SET(CMAKE_C_FLAGS "${MCFLAGS} ${TOPT} ${OPT} ${COPT} ${CWARN} ${DEFS}" )
#SET(CMAKE_CXX_FLAGS "${MCFLAGS} ${TOPT} ${OPT} ${CXXOPT} ${CXXWARN} ${DEFS}" )


add_library(minilouge-xd-platform INTERFACE EXCLUDE_FROM_ALL)

# TODO: specifying map here doesn't make sense
target_link_options(minilouge-xd-platform INTERFACE
        ${MCFLAGS} ${TOPT} ${OPT} -nostartfiles -Wl,-Map=${CMAKE_CURRENT_BINARY_DIR}/${PROJECT}.map,--cref,--no-warn-mismatch,--library-path=${RULESPATH},--script=${LDSCRIPT} ${LDOPT}
        )

# TODO: split C and C++ compile options
target_compile_options(minilouge-xd-platform INTERFACE
        ${MCFLAGS} ${TOPT} ${OPT} ${CXXOPT} ${CXXWARN} ${DEFS}
        )


target_include_directories(minilouge-xd-platform INTERFACE
        ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/inc
        ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/inc/dsp
        ${logue-sdk_SOURCE_DIR}/platform/minilogue-xd/inc/utils
        ${logue-sdk_SOURCE_DIR}/platform/ext/CMSIS/CMSIS/Include
        )