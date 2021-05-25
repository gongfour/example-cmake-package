cmake_minimum_required(VERSION 3.0.0)
project(cmake-package VERSION 0.1.0)

add_library(cmake-package cmake-package.cpp)
# target_include_directories(${PROJECT_NAME} 
#     PUBLIC 
#     $<INSTALL_INTERFACE:${PROJECT_SOURCE_DIR}>/include
# )

# INSTALL Targets
install(TARGETS cmake-package EXPORT CustomTargets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

# INSTALL PUBLIC Headers
install(
    FILES
        cmake-package.h
    DESTINATION
        include
)

#
# PACKAGE
#

# ConfigVersion.cmake를 만들어주기 위해 사용 (매크로)
# <PackageName>_VERSION
# <PackageName>_VERSION_MAJOR
# <PackageName>_VERSION_MINOR 등
# include(CMakePackageConfigHelpers) 

# 이 명령어는 CMake Target을 외부 프로젝트에서 사용하기 위해 export 하는 명령어이다.
# 이 명령어만 사용했을 시 절대경로로 프로젝트가 구성되기 때문에 해당 PC에서만 사용할 수 있다.
# export(EXPORT CustomTargets 
#     FILE ${CMAKE_CURRENT_BINARY_DIR}/Custom-CP/Custom-CPTargets.cmake
# )

# install(EXPORT) 명령어는 install(TARGETS)에 정의된 export-set을 내보내는데 사용됨
# 이 명령어는 IMPORTED target을 포함하는 <PackageName>Target.cmake 파일을 만듬
install(EXPORT CustomTargets
    FILE
        Custom-CPTargets.cmake # 생성할 파일의 이름
    DESTINATION  
        lib/cmake/Custom-CP # 파일이 생성 될 위치
)

# 해당 명령어로 <PackageName>Target.cmake를 import 하는 <Package>Config.cmake를 install함
install(
    FILES
        cmake/Custom-CPConfig.cmake
    DESTINATION
        lib/cmake/Custom-CP
)

# PACKAGE NSIS
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
