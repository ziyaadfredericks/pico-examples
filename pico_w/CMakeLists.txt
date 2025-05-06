# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico_w CACHE STRING "Board type")

cmake_minimum_required(VERSION 3.12)

if (PICO_CYW43_SUPPORTED) # set by PICO_BOARD=pico_w
    if (NOT TARGET pico_cyw43_arch)
        message("Skipping Pico W examples as support is not available")
    else()

        if (DEFINED ENV{WIFI_SSID} AND (NOT WIFI_SSID))
            set(WIFI_SSID $ENV{WIFI_SSID})
            message("Using WIFI_SSID from environment ('${WIFI_SSID}')")
        endif()

        if (DEFINED ENV{WIFI_PASSWORD} AND (NOT WIFI_PASSWORD))
            set(WIFI_PASSWORD $ENV{WIFI_PASSWORD})
            message("Using WIFI_PASSWORD from environment")
        endif()

        set(WIFI_SSID "${WIFI_SSID}" CACHE INTERNAL "WiFi SSID for examples")
        set(WIFI_PASSWORD "${WIFI_PASSWORD}" CACHE INTERNAL "WiFi password for examples")

        add_subdirectory(wifi)
        if (NOT TARGET pico_btstack_base)
            message("Skipping Pico W Bluetooth examples as support is not available")
        else()
            add_subdirectory(bt)
        endif()
    endif()
endif()
