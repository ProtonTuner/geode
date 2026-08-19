cmake_minimum_required(VERSION 3.21)
project(PhysicsTPS VERSION 1.0.1 LANGUAGES CXX)

# Detectar plataforma
if(WIN32)
    set(GEODE_TARGET_PLATFORM win)
elseif(APPLE)
    set(GEODE_TARGET_PLATFORM mac)
elseif(ANDROID)
    set(GEODE_TARGET_PLATFORM android)
else()
    message(FATAL_ERROR "Plataforma no soportada")
endif()

# Configuración de C++
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Incluir Geode
include(FetchContent)
FetchContent_Declare(
    geode
    GIT_REPOSITORY https://github.com/geode-sdk/geode.git
    GIT_TAG v3.0.0
)
FetchContent_MakeAvailable(geode)

# Crear el mod
geode_add_mod(
    ${PROJECT_NAME}
    VERSION 1.0.1
    ID zmx.cbf-lite
)

# Archivos fuente
target_sources(
    ${PROJECT_NAME}
    PRIVATE
    src/main.cpp
    src/main.hpp
)

# Plataforma específica
if(GEODE_TARGET_PLATFORM STREQUAL "windows")
    target_sources(${PROJECT_NAME} PRIVATE src/windows.cpp)
elseif(GEODE_TARGET_PLATFORM STREQUAL "android")
    target_sources(${PROJECT_NAME} PRIVATE src/android.cpp)
endif()

# Optimizaciones
target_compile_options(${PROJECT_NAME} PRIVATE
    $<$<CONFIG:Release>:-O3>
    $<$<CONFIG:RelWithDebInfo>:-O2>
)

# Configuración del mod.json
set_target_properties(${PROJECT_NAME} PROPERTIES
    MOD_ID "zmx.cbf-lite"
    MOD_NAME "Physics TPS Bypass"
    MOD_VERSION "1.0.1"
    MOD_DESCRIPTION "Fuerza la fisica del jugador a una tasa fija configurable"
)
