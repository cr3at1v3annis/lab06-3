cmake_minimum_required(VERSION 3.10) #минимально требуемая версия для для сборки CMake
project(formatter_ex) # название проекта

set(CMAKE_CXX_STANDARD 11) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов
set(CMAKE_CURRENT_SOURCE_DIR /home/annis/TiMP/lab03) # изменение переменной CMAKE_CURRENT_SOURCE_DIR

add_library(formatter_ex STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp) # сборка библиотеки

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}
        \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib) # добавление директории где находится исполняемые файлы

target_link_libraries(formatter formatter_ex) # указания, где находятся директории
