cmake_minimum_required(VERSION 3.21)
project(PhysicsTPS VERSION 1.0.1 LANGUAGES CXX)

# Configuración de C++
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

# Incluir Geode SDK
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
    ID paradox.physicstps
)

# Archivos fuente comunes
target_sources(
    ${PROJECT_NAME}
    PRIVATE
    src/main.cpp
    src/main.hpp
)

# Archivos específicos por plataforma
if(GEODE_PLATFORM_WINDOWS)
    target_sources(${PROJECT_NAME} PRIVATE src/windows.cpp)
    message(STATUS "Building for Windows")
elseif(GEODE_PLATFORM_MACOS)
    target_sources(${PROJECT_NAME} PRIVATE src/macos.cpp)
    message(STATUS "Building for macOS")
elseif(GEODE_PLATFORM_ANDROID)
    target_sources(${PROJECT_NAME} PRIVATE src/android.cpp)
    message(STATUS "Building for Android")
endif()

# Optimizaciones
target_compile_options(${PROJECT_NAME} PRIVATE
    $<$<CONFIG:Release>:-O3>
)

message(STATUS "Physics TPS Bypass configured for ${GEODE_PLATFORM}")
