cmake_minimum_required(VERSION 3.1)

set(project_name sample) ## rename your project here

project(${project_name})

set(CMAKE_CXX_STANDARD 11)

add_library(${project_name}-lib
        src/AppComponent.hpp
        src/controller/MyController.cpp
        src/controller/MyController.hpp
        src/dto/DTOs.hpp
)

## link libs

#find_package(oatpp 1.1.0 REQUIRED)
add_subdirectory(lib/oatpp)
add_subdirectory(lib/oatpp-libressl)

target_link_libraries(${project_name}-lib
        PUBLIC oatpp
        PUBLIC oatpp-libressl
)

target_include_directories(${project_name}-lib PUBLIC src)

## add executables

add_executable(${project_name}
        src/App.cpp
        test/app/MyApiTestClient.hpp)
target_link_libraries(${project_name} ${project_name}-lib)
add_dependencies(${project_name} ${project_name}-lib)

#add_executable(${project_name}-test
#        test/tests.cpp
#        test/app/TestComponent.hpp
#        test/app/MyApiTestClient.hpp
#        test/MyControllerTest.cpp
#        test/MyControllerTest.hpp
#)

#target_link_libraries(${project_name}-test ${project_name}-lib)
#add_dependencies(${project_name}-test ${project_name}-lib)

set_target_properties(${project_name}-lib ${project_name} PROPERTIES
        CXX_STANDARD 11
        CXX_EXTENSIONS OFF
        CXX_STANDARD_REQUIRED ON
)

#enable_testing()
#add_test(project-tests ${project_name}-test)
