## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.
```bush

$ cmake --version
cmake version 3.29.3

CMake suite maintained and supported by Kitware (kitware.com/cmake).

```

```bush

cd formatter_lib
cat >> CMakeLists.txt << EOF
>cmake_minimum_required(VERSION 3.29.3)
>
>project(formatter_lib)
>
>set(CMAKE_CXX_STANDARD 20)
>set(CMAKE_CXX_STANDARD_REQUIRED ON)
>
>add_library(formatter_lib STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
> 
> include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
> 
> EOF

cmake -H. -B build

```

## Вывод:

```bush
-- The C compiler identification is AppleClang 15.0.0.15000309
-- The CXX compiler identification is AppleClang 15.0.0.15000309
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.8s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/aleksandrnikonorov/Azuhonahus/workspace/tasks/lab03/formatter_lib/build

```
## Файл CMakeLists

```
cmake_minimum_required(VERSION 3.29.3)

project(formatter_lib)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

```bush
cd formatter_ex_lib
 cat>> CMakeLists.txt << EOF
>cmake_minimum_required(VERSION 3.29.3)
>
>project(formatter_ex_lib)
>
>set(CMAKE_CXX_STANDART 20)
>set(CMAKE_CXX_STANDART_REQUIRED ON)
>set(CMAKE_CURRENT_SOURCE_DIR /Users/aleksandrnikonorov/Azuhonahus/workspace/tasks/lab03/formatter_ex_lib)
>
>add_library(formatter_ex STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
>
>include_directories(${CMAKE_CURRENT_SOURCE_DIR})
>include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
>
>target_link_libraries(formatter_ex formatter)
> EOF

cmake -H. -B build

```

## Вывод:

```bush
-- The C compiler identification is AppleClang 15.0.0.15000309
-- The CXX compiler identification is AppleClang 15.0.0.15000309
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.6s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/aleksandrnikonorov/Azuhonahus/workspace/tasks/lab03/formatter_ex_lib/build

```


## Файл CMakeLists

```
cmake_minimum_required(VERSION 3.22.1)

project(formatter_ex_lib)

set(CMAKE_CXX_STANDART 20)
set(CMAKE_CXX_STANDART_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/aleksandrnikonorov/Azuhonahus/workspace/tasks/lab03/formatter_ex_lib)

add_library(formatter_ex STATIC /formatter_ex.cpp)

include_directories()
include_directories(/formatter_lib)

target_link_libraries(formatter_ex formatter)

```


### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;


```bush
cd ../hello_world_application

cat>> CMakeLists.txt << EOF
> cmake_minimum_required(VERSION 3.29.3)
>project(hello_world)
>
>set(CMAKE_CXX_STANDARD 20)
>set(CMAKE_CXX_STANDARD_REQUIRED ON)
>
>add_executable(hello_world hello_world.cpp)
>
>add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
>add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)
>
>
>target_include_directories(formatter_lib PUBLIC ../formatter_lib)
>target_include_directories(formatter_ex_lib PUBLIC ../formatter_ex_lib ../formatter_lib)
>target_include_directories(hello_world PUBLIC ../formatter_ex_lib ../formatter_lib)
>
>target_link_libraries(hello_world formatter_ex_lib formatter_lib)
> EOF


cmake -H. -B build

```

## Вывод:

```bush
-- The C compiler identification is AppleClang 15.0.0.15000309
-- The CXX compiler identification is AppleClang 15.0.0.15000309
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.6s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/aleksandrnikonorov/Azuhonahus/workspace/tasks/lab03/hello_world_application/build


```

## Файл CMakeLists

```

cmake_minimum_required(VERSION 3.29.3)
project(hello_world)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(hello_world hello_world.cpp)

add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)


target_include_directories(formatter_lib PUBLIC ../formatter_lib)
target_include_directories(formatter_ex_lib PUBLIC ../formatter_ex_lib ../formatter_lib)
target_include_directories(hello_world PUBLIC ../formatter_ex_lib ../formatter_lib)

target_link_libraries(hello_world formatter_ex_lib formatter_lib)

```

# 2. Solver_application
```
$ cd ../solver_lib/
$ cat >> ./CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.29.3)
project(solver)
EOF
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/aleksandrnikonorov/Azuhonahus/workspace/projects/lab03)
EOF
$ cat >> ./CMakeLists.txt << EOF
add_library(solver_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
EOF
$ cat >> ./CMakeLists.txt << EOF
include_directories(/solver_lib /formatter_lib /formatter_ex_lib)
EOF
$ cat >> CMakeLists.txt << EOF
add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
EOF
$ cat >> CMakeLists.txt << EOF
find_library(formatter NAMES libformatter.a PATHS ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
find_library(solver_lib NAMES libsolver.a PATHS ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
 EOF
$ cat >> CMakeLists.txt << EOF
target_link_libraries(solver ${formatter} ${formatter_ex} ${solver_lib})
EOF
$ cmake -H. -B_build
```
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.29.3)

project(solver)

set(SOURCE_EXE equation.cpp)

include_directories("${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/"
					"${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/")

add_executable(main ${SOURCE_EXE})

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/ formatter_ex_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/ solver_lib)

target_link_libraries(main formatter_ex_lib solver_lib)
```
# 2. Solver_lib
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.29.3)

project(solver_lib)

set(SOURCE_LIB solver.cpp solver.h)

add_library(solver_lib STATIC ${SOURCE_LIB})
```
