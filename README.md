# LearnCmake
1. 内部构建：直接在根目录开干。   
外部构建：例如: `cd build; make;`

2. 编译路径：`PROJECT_BINARY_DIR = lala/build`   
工程顶层路径：`lala`   
`CMAKE_CURRENT_SOURCE_DIR`，当前处理的 **CMakeLists.txt** 所在的路径。

3. 头文件包含   
例：`include_directories(./include)`，把当前目录（CMakeLists.txt 所在目录）下的 include 目录添加到包含路径。   
`include_directories(${CMAKE_CURRENT_LIST_DIR}/include)` 一样的效果。

4. 库文件包含   
`link_directories(./lib)`

5. 链接文件   
`target_link_libraries(${PROJECT_NAME} util)`，名字叫 `${PROJECT_NAME}` 这个 **target** 需要链接 **util** 这个库，先搜索 **libutil.a** ，没有就搜 **libutil.so** 。

6. 传 FLAGS 给编译器   
例：`set(CMAKE_CXX_FLAGS "-g")`

7. 添加子目录   
`add_subdirectory(source_dir [binary_dir])`，将一个子目录添加到构建中。   
**source_dir**指定源 **CMakeLists.txt** 和代码目录。**binary_dir** 指定输出目录，如果没有则会输出到当前构建目录（例如 build 目录）。

8. 换个地方保存目标   
`set(CMAKE_RUNTIME_OUTPUT_DIRECTORY lala/bin)`   
`SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY lala/lib)`   
老的版本是 **EXECUTABLE_OUTPUT_PATH**

9. `set_target_properties(target1 target 2 ... PROPERTIES prop1 value1 prop2 value2 ...)`   

10. **rpath**   
**RPATH**，其实就是硬编码在可执行文件或动态库中的一个或者多个路径，被动态链接器用来搜索依赖库。值存在 **ELF** 结构中的 **.dynamic** 小节中。可以用 `readelf -d a.out | grep RPATH` 查看。   
链接加载器的路径搜索顺序为：**RPATH -> LD_LIBRARY_PATH -> ...**   
有时候在 sh 脚本文件中可以看到：`LD_LIBRARY_PATH=/opt/shared/lib ./Bifrost > 1.log 2>&|&`   