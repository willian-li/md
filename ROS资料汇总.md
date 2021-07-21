# ROS资料汇总

## 1. 参考资料

[官方文档教程](http://wiki.ros.org/cn/ROS/Tutorials)

[古月居视频入门21讲](https://www.bilibili.com/video/BV1zt411G7Vn?from=search&seid=10794056852179208332)

[ROS机器人入门课程](https://www.bilibili.com/video/BV1Ci4y1L7ZZ)   ---->[对应课程文档](http://www.autolabor.com.cn/book/ROSTutorials/)





## 2. ROS入门

### 安装 ros

```java
sudo rosdep init ERROR
```

解决：[参考博客](https://blog.csdn.net/weixin_43619346/article/details/106974715)

---

### 文件系统工具

常用命令：

1. rospack

```java
rospack find [package_name] //获取功能包路径
```

2. roscd

```java
roscd [package_name]       //直接切换到功能包路径
roscd [package_name]/camke //也可以切换到一个功能包或功能包集的子目录中
roscd log                  //进入储存ROS日志文件的目录
```

3. rosls

```java
rosls [package_name]       //显示功能包下的文件
```

4. Tab补全

```java
roscd roscpp_tut<<<按TAB键>>>
roscd roscpp_tutorials/   //命令行自动补全剩余部分
```

```java
roscd tur<<<按TAB键>>>
roscd turtle    //补全    
```

然而，在这种情况下有许多软件包都以`turtle`开头。当再次按**TAB**键后会列出所有以`turtle`开头的ROS软件包：

```java
turtle_actionlib/  turtlesim/         turtle_tf/
```

这时在命令行中你仍然只输入了：

```java
roscd turtle
```

现在在`turtle`后面输入`s`然后按**TAB**键：

```java
roscd turtles<<<按TAB键>>>
```

因为只有一个软件包的名称以`turtles`开头，所以你应该会看到：

```java
roscd turtlesim/
```

如果要查看当前安装的所有软件包的列表，你也可以利用TAB补全：

```java
rosls <<<双击TAB键>>>
```

---

### 创建ROS功能包

1. `catkin`功能包组成

最简单的功能包组成:

```java
my_package/
  CMakeLists.txt //提供有关该软件包的元信息
  package.xml
```



### 使用roslaunch

首先切换到指定路径，然后`rolaunch`+`文件名`

```java
roscd roslaunch
roslaunch example.launch
```

或者 `roslaunch`+`功能包名`+`文件名`

```java
 roslaunch roslaunch example.launch
```



`roslaunch`可以用来启动定义在`launch（启动）`文件中的节点。

用法：

```java
$ roslaunch [package] [filename.launch]
```

**注意：**存放launch文件的目录不一定非要命名为`launch`，事实上都不用非得放在目录中，`roslaunch`命令会自动查找经过的包并检测可用的启动文件。然而，这种推荐的标准做法被认为是“最佳实践”。

### launch解析

下面我们开始拆解launch XML文件。

```xml
   1 <launch>
```

首先用launch标签开头，以表明这是一个launch文件。

```xml
   3   <group ns="turtlesim1">
   4     <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
   5   </group>
   6 
   7   <group ns="turtlesim2">
   8     <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
   9   </group>
```

此处我们创建了两个分组，并以命名空间（namespace）标签来区分，其中一个名为`turtulesim1`，另一个名为`turtlesim2`，两个分组中都有相同的名为`sim`的turtlesim节点。这样可以让我们同时启动两个turtlesim模拟器，而不会产生命名冲突。

```xml
  11   <node pkg="turtlesim" name="mimic" type="mimic">
  12     <remap from="input" to="turtlesim1/turtle1"/>
  13     <remap from="output" to="turtlesim2/turtle1"/>
  14   </node>
```

在这里我们启动模仿节点，话题的输入和输出分别重命名为`turtlesim1`和`turtlesim2`，这样就可以让turtlesim2模仿turtlesim1了。

```xml
  16 </launch>
```

这一行使得launch文件的XML标签闭合。