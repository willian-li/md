# roslaunch的使用

### roslaunch命令行

#### 运行launch文件

首先切换到指定路径，然后`rolaunch`+`文件名`

```java
roscd roslaunch
roslaunch example.launch
```

或者 `roslaunch`+`功能包名`+`文件名`

```java
 roslaunch roslaunch example.launch
```



roslaunch <package-name> <launch-filename> [args]

Launch `<launch-filename>` located in `<package-name>`, e.g.:

```
roslaunch rospy_tutorials talker_listener.launch
```

`roslaunch` will find a file with the matching name inside the specified package and run it.

```
roslaunch <launch-file-paths...> [args]
```

- Launch the file(s) specified by relative or absolute paths, e.g.

  ```
  $ roslaunch pr2_robot/pr2_bringup/pr2.launch
  ```

### XML

#### 评估顺序

一个参数有多个设置，将使用为该参数指定的最后一个值。

建议使用`（arg）` / `<arg>`设置来执行覆盖行为

#### 替代参数

Roslaunch tag attributes can make use of *substitution args*, which roslaunch will resolve prior to launching nodes. The currently supported substitution args are:

- `$(env ENVIRONMENT_VARIABLE)`

  - Substitute the value of a variable from the current environment. The launch will fail if environment variable is not set. This value cannot be overridden by `<env>` tags.

  `$(optenv ENVIRONMENT_VARIABLE)` `$(optenv ENVIRONMENT_VARIABLE default_value)`

  - Substitute the value of an environment variable if it is set. If `default_value` is provided, it will be used if the environment variable is not set. If `default_value` is not provided, an empty string will be used. `default_value` can be multiple words separated by spaces.

- Examples:

  

  - 

    

    

    

    ```
      <param name="foo" value="$(optenv NUM_CPUS 1)" />
      <param name="foo" value="$(optenv CONFIG_PATH /home/marvin/ros_workspace)" />
      <param name="foo" value="$(optenv VARIABLE ros rocks)" />
    ```

    

  `$(find pkg)`

  - e.g. `$(find rospy)/manifest.xml`. Specifies a *package-relative path*. The filesystem path to the package directory will be substituted inline. Use of package-relative paths is highly encouraged as hard-coded paths inhibit the portability of the launch configuration. Forward and backwards slashes will be resolved to the local filesystem convention.

  `$(anon name)`

  - e.g. `$(anon rviz-1)`. Generates an anonymous id based on `name`. `name` itself is a unique identifier: multiple uses of `$(anon foo)` will create the same "anonymized" [name](http://wiki.ros.org/Names). This is used for <node> `name` attributes in order to create [nodes](http://wiki.ros.org/Nodes) with anonymous [names](http://wiki.ros.org/Names), as ROS requires nodes to have unique names. For example:

    ```
      <node name="$(anon foo)" pkg="rospy_tutorials" type="talker.py" />
      <node name="$(anon foo)" pkg="rospy_tutorials" type="talker.py" />
    ```

    

    Will generate an error that there are two <node>s with the same name.

  `$(arg foo)`

  - `$(arg foo)` evaluates to the value specified by an [](http://wiki.ros.org/roslaunch/XML/arg) tag. There must be a corresponding [](http://wiki.ros.org/roslaunch/XML/arg) tag *in the same launch file* that declares the arg.

  - For example:

    

    

    

    ```
      <param name="foo" value="$(arg my_foo)" />
    ```

    

    

  - Will assign the *my_foo* argument to the *foo* parameter.

  - Another example:

    

    

    

    

    ```
    <node name="add_two_ints_server" pkg="beginner_tutorials" type="add_two_ints_server" />
    <node name="add_two_ints_client" pkg="beginner_tutorials" type="add_two_ints_client" args="$(arg a) $(arg b)" />
    ```

    

    Will launch both the server and client from the [](http://wiki.ros.org/ROS/Tutorials/WritingServiceClient(c%2B%2B)) example, passing as parameters the value *a* and *b*. The resulting launch project can be called as follows:

    ```
    roslaunch beginner_tutorials launch_file.launch a:=1 b:=5
    ```

    

  `$(eval <expression>)` **New in Kinetic**

  - `$(eval <expression>)` allows to evaluate arbitrary complex python expressions.

  - For example:

    

    

    

    ```
    <param name="circumference" value="$(eval 2.* 3.1415 * arg('radius'))"/>
    ```

    

    

  - Will compute the circumference from the *radius* argument and assign the result to an appropriate parameter.

  - **Note**: As a limitation, `$(eval)` expressions need to span the whole attribute string. A mixture of other substitution args with `eval` within a single string is **not** possible:

    ```
    <param name="foo" value="$(arg foo)$(eval 6*7)bar"/>
    ```

    

    To remedy this limitation, all substitution commands are available as functions *within* `eval` as well:

    ```
    "$(eval arg('foo') + env('PATH') + 'bar' + find('pkg')"
    ```

    

    

  - For your convenience, arguments are also implicitly resolved, i.e. the following two expressions are identical:

    

    

    

    

    ```
    "$(eval arg('foo'))"
    "$(eval foo)"
    ```

    

    

  `$(dirname)` **New in Lunar**

  - `$(dirname)` returns the absolute path to the directory of the launch file in which it appears. This can be used in conjunction with `eval` and `if/unless` to modify behaviour based on the installation path, or simply as a convenience for referencing `launch` or `yaml` files relative to the current file rather than relative to a package root (as with `$(find PKG)`).

  - For example:

    

    

    

    ```
    <include file="$(dirname)/other.launch" />
    ```

    

    

  - Will expect to find `other.launch` in the same directory as the launch file which it appears in.

**Substitution args are currently resolved on the local machine.** In other words, environment variables and ROS package paths will be set to their values in your current environment, **even for remotely launched processes**.

#### 如何和除非属性

All tags support `if` and `unless` attributes, which include or exclude a tag based on the evaluation of a value. "1" and "true" are considered true values. "0" and "false" are considered false values. Other values will error.

- `if=value` (optional)

  - If `value` evaluates to true, include tag and its contents.

  `unless=value` (optional)

  - Unless `value` evaluates to true (which means if `value` evaluates to false), include tag and its contents.

#### 标签参考

