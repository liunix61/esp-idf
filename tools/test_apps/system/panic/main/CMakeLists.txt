set(srcs "test_app_main.c" "test_panic.c")

if(CONFIG_TEST_MEMPROT)
    list(APPEND srcs "test_memprot.c")
    if(CONFIG_SOC_MEMPROT_SUPPORTED)
        if(CONFIG_IDF_TARGET_ARCH_XTENSA)
            list(APPEND srcs "panic_utils/memprot_panic_utils_xtensa.c")
        elseif(CONFIG_IDF_TARGET_ARCH_RISCV)
            list(APPEND srcs "panic_utils/memprot_panic_utils_riscv.c")
        endif()
    endif()
endif()

idf_component_register(SRCS "${srcs}"
                        INCLUDE_DIRS "include"
                        REQUIRES spi_flash esp_psram esp_system esp_partition espcoredump
                        PRIV_REQUIRES esp_gdbstub)

target_compile_options(${COMPONENT_LIB} PRIVATE "-Wno-unused-variable"
                                                "-Wno-infinite-recursion"
                                                "-Wno-array-bounds")
