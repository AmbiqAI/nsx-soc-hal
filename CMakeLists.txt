option(NSX_AMBIQSUITE_USE_PREBUILT "Use prebuilt AmbiqSuite static libraries" ON)
option(NSX_AMBIQSUITE_USE_SOURCE_FALLBACK "Use local source fallback if prebuilt is disabled" ON)

if(NOT DEFINED NSX_BOARD_FLAGS_TARGET)
    message(FATAL_ERROR "Board must define NSX_BOARD_FLAGS_TARGET before ambiqsuite_core is added.")
endif()

add_library(nsx_soc_hal INTERFACE)
set_target_properties(nsx_soc_hal PROPERTIES EXPORT_NAME ${NSX_SOC_TARGET_EXPORT_NAME})
add_library(nsx::soc_hal ALIAS nsx_soc_hal)
if(NSX_SOC_FAMILY STREQUAL "apollo3p")
    add_library(nsx::soc_hal_apollo3p ALIAS nsx_soc_hal)
elseif(NSX_SOC_FAMILY STREQUAL "apollo4p")
    add_library(nsx::soc_hal_apollo4p ALIAS nsx_soc_hal)
else()
    add_library(nsx::soc_hal_apollo510 ALIAS nsx_soc_hal)
endif()
add_library(nsx::ambiqsuite_core ALIAS nsx_soc_hal)

target_link_libraries(nsx_soc_hal INTERFACE ${NSX_BOARD_FLAGS_TARGET})
if(NOT TARGET nsx_ambiq_hal)
    message(FATAL_ERROR "ns-soc-hal requires a release-specific ambiq HAL module to be added first.")
endif()
if(NOT TARGET nsx_ambiq_bsp)
    message(FATAL_ERROR "ns-soc-hal requires a release-specific ambiq BSP module to be added first.")
endif()
target_link_libraries(nsx_soc_hal INTERFACE nsx_ambiq_bsp nsx_ambiq_hal)

install(TARGETS
    nsx_soc_hal
    EXPORT nsxTargets
)
