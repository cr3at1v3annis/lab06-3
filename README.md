## Laboratory work III и IV

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake** и **GitHub Actions**



## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.   
`CMakeList.txt` для  [formatter_lib](formatter_lib)  
```sh
cmake_minimum_required(VERSION 3.4) #минимально требуемая версия для для сборки CMake
project(formatter) # название проекта

set(CMAKE_CXX_STANDARD 11) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов

add_library(formatter STATIC formatter.cpp formatter.h)
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.  
`CMakeList.txt` для  [formatter_ex_lib](formatter_ex__lib) 
 ```sh
cmake_minimum_required(VERSION 3.4) #минимально требуемая версия для для сборки CMake
project(formatter_ex) # название проекта

set(CMAKE_CXX_STANDARD 11) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов

add_library(formatter_ex STATIC formatter_ex.cpp formatter_ex.h) # сборка библиотеки

target_include_directories(formatter_ex PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib)

target_link_libraries(formatter_ex formatter)
 ``` 


### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.    
`CMakeList.txt` для  *hello_world*
 ```sh
cmake_minimum_required(VERSION 3.4) #минимально требуемая версия для для сборки CMake
project(hello_world) # название проекта

set(CMAKE_CXX_STANDARD 11) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов

add_executable(hello_world hello_world.cpp)
target_include_directories(hello_world PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib)
target_link_libraries(hello_world formatter_ex) # указания, где находятся директории
 ``` 
 `CMakeList.txt` для  *solver_lib*
 ```sh
cmake_minimum_required(VERSION 3.4)
project(solver)

set(CMAKE_CXX_STANDARD 20) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов

add_library(solver STATIC solver.cpp solver.h)
 ``` 
 `CMakeList.txt` для  *solver_application*
 ```sh
cmake_minimum_required(VERSION 3.4)
project(solver)

set(CMAKE_CXX_STANDARD 11) # установка стандартов
set(CMAKE_CXX_STANDARD_REQUIRED ON) # установка стандартов
add_executable(solve equation.cpp)
target_include_directories(solve PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib)
target_include_directories(solve PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib)
target_link_libraries(solve formatter_ex)
target_link_libraries(solve solver)
 ``` 
`CMakeList.txt` для  всего приложения
 ```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(cmake)

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(hello_world_application)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)
 ``` 
### Задание Лабораторной работы № 4
`cmake.yml` для  всего приложения
 ```sh
name: Formatter_app

on:
  push:
    branches: 
    - master

jobs:
  libraries:
    
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: FormatterLib CMake
      shell: bash
      run: |
        cd formatter_lib
        cmake -H. -B_build
        cmake --build _build
    - name: FormatterExLib CMake
      shell: bash
      run: |
        cd formatter_ex_lib
        cmake -H. -B_build
        cmake --build _build
        
    - name: SolverLib Cmake
      shell: bash
      run: |
        cd solver_lib
        cmake -H. -B_build
        cmake --build _build
        
  Hello_world:
  
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
      
    - name: HelloWorldApplication CMake
      shell: bash
      run: |
        mkdir _build
        cd _build
        cmake ..
        cmake --build .
        cd hello_world_application
        ./hello_world
        
  Solver:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
      
    - name: SolverApplication CMake
      shell: bash
      run: |
        mkdir _build2
        cd _build2
        cmake ..
        cmake --build .
        cd solver_application
        ./solve 2 -4 2
 ``` 

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2021 The ISC Authors
```
