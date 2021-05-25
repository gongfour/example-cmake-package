cmake_minimum_required(VERSION 3.0.0)
project(cmake-package VERSION 0.1.0)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_INCLUDE_CURRENT_DIR_IN_INTERFACE ON)

add_library(cmake-package cmake-package.cpp)
# target_include_directories(${PROJECT_NAME} 
#     PUBLIC 
#     $<INSTALL_INTERFACE:${PROJECT_SOURCE_DIR}>/include
# )

# INSTALL
install(TARGETS cmake-package EXPORT CustomTargets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)
install(
    FILES
        cmake-package.h
    DESTINATION
        include
)

#
# PACKAGE (FILE 위치로 Custom-CP.cmake를 내보냄)
#

# ConfigVersion.cmake를 만들어주기 위해 사용 (매크로)
# <PackageName>_VERSION
# <PackageName>_VERSION_MAJOR
# <PackageName>_VERSION_MINOR 등
include(CMakePackageConfigHelpers) 

export(EXPORT CustomTargets 
    FILE ${CMAKE_CURRENT_BINARY_DIR}/Custom-CP/Custom-CPTargets.cmake
    NAMESPACE Custom-CP::
)

# install(EXPORT) 명령어는 install(TARGETS)에 정의된 export-set을 내보내는데 사용됨
# 이 명령어는 IMPORTED target을 포함하는 <PackageName>Target.cmake 파일을 만듬
set(ConfigPackageLocation lib/cmake/Custom-CP)
install(EXPORT CustomTargets
    FILE
        Custom-CPTargets.cmake
    DESTINATION  
        ${ConfigPackageLocation}
)

# PACKAGE NSIS
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
