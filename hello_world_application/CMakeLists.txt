cmake_minimum_required(VERSION 3.10) #минимально требуемая версия для для сборки CMake
project(hello_world) # название проекта

set(CMAKE_CXX_STANDARD 20) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов
set(CMAKE_CURRENT_SOURCE_DIR /home/annis/TiMP/lab03) # изменение переменной CMAKE_CURRENT_SOURCE_DIR

add_executable(hello_world \${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)


include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib
        \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib) # добавление директории где находится исполняемые файлы
find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_DIR}/formatter_ex_lib)

target_link_libraries(hello_world \${formatter} \${formatter_ex}) # указания, где находятся директории