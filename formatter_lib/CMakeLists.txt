cmake_minimum_required(VERSION 3.10) #минимально требуемая версия для для сборки CMake
project(formatter) # название проекта

set(CMAKE_CXX_STANDARD 20) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов

add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp) # добавление библиотеки

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}) # добавление директории где находится исполняемые файлы

set_property(TARGET ... PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
