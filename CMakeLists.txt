cmake_minimum_required(VERSION 3.0.0)
project(cmake-package VERSION 0.1.0)

include(CTest)
enable_testing()

add_library(cmake-package cmake-package.cpp)
target_include_directories(${PROJECT_NAME} PUBLIC .)

# INSTALL
install(TARGETS cmake-package EXPORT Custom-CP)

# PACKAGE (FILE 위체로 Custom-CP.cmake를 내보냄)
include(CMakePackageConfigHelpers)
export(EXPORT Custom-CP FILE ${CMAKE_CURRENT_BINARY_DIR}/Custom-CP/Custom-CPConfig.cmake)
install(
    FILES
        ${CMAKE_CURRENT_BINARY_DIR}/Custom-CP/Custom-CPConfig.cmake
    DESTINATION  
        lib/cmake/Custom-CP
)

# PACKAGE NSIS
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
