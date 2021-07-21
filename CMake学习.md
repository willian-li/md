CMakeLists.txt文件，是cmake的构建定义文件

1. `cmake .`编译本目录 `cmake ..`编译上一级目录

2. 生成关键文件`Makefile` 

3. `make`工程实际构建



三个指令：

```
PROJECT：定义工程名称
MESSAGE
ADD_EXECUTABLE
```



基本语法规则：

1,变量使用${}方式取值,但是在 IF 控制语句中是直接使用变量名
		2,指令(参数 1 参数 2...)

使用外部编译

建一个目录

`cmake ..`

`make`

```
ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
这个指令用于向当前工程添加存放源文件的子目录,并可以指定中间二进制和目标二进制存
放的位置。
```

