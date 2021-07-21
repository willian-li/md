# launch文件简单使用

### 使用roslaunch

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