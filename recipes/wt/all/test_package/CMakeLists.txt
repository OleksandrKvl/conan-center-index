cmake_minimum_required(VERSION 3.15)
project(test_package LANGUAGES CXX)

add_executable(${PROJECT_NAME} test_package.cpp)

set(WT_COMPONENTS Wt HTTP)
set(WT_TARGETS Wt::Wt Wt::HTTP)
if(TARGET Wt::Dbo)
    target_compile_definitions(${PROJECT_NAME} PRIVATE WITH_DBO)
    list(APPEND WT_COMPONENTS Dbo)
    list(APPEND WT_TARGETS Wt::Dbo)
endif()

find_package(wt REQUIRED ${WT_COMPONENTS} CONFIG)
target_link_libraries(${PROJECT_NAME} PRIVATE ${WT_TARGETS})

if(wt_VERSION VERSION_LESS "4.5.0")
    target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_11)
else()
    target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_14)
endif()
