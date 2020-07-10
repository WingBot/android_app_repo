# android_app_repo





```bash
$ cd ws_android_app/src
$ git clone https://github.com/ros-planning/warehouse_ros.git
$ git clone https://github.com/ros-planning/warehouse_ros_mongo.git
$ git clone https://github.com/corot/world_canvas.git

$ cd ~/3th-party-lib
$ git clone -b 26compat https://github.com/mongodb/mongo-cxx-driver.git
$ sudo apt-get install scons
$ cd ~/3th-party-lib/mongo-cxx-driver
$ sudo scons --prefix=/usr/local/ --full --use-system-boost --disable-warnings-as-errors
$ cd 
$ cd catkin_ws
$ catkin_make
```

编译成功

### 运行:

```bash
$ source ~/ws_
$ roslaunch world_canvas_server world_canvas_server.launch debug:=true --screen
```

### 问题1:

> File "/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_server/scripts/world_canvas_server", line 6, in <module>
>  import world_canvas_server
> File "/home/ars-lab/ws_android_app/devel/lib/python2.7/dist-packages/world_canvas_server/__init__.py", line 34, in <module>
>  exec(__fh.read())
> File "<string>", line 1, in <module>
> File "/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_server/src/world_canvas_server/annotations_server.py", line 40, in <module>
>  import warehouse_ros as wr
> ImportError: No module named warehouse_ros

![image-20200709181549060](/home/ars-lab/zech_ws/NutNote/Typora data/image-20200709181549060.png)

### 解决:

修改对应文件对应行号加_mongo

### 问题2:

```bash
  File "/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_server/scripts/world_canvas_server", line 6, in <module>
    import world_canvas_server
  File "/home/ars-lab/ws_android_app/devel/lib/python2.7/dist-packages/world_canvas_server/__init__.py", line 34, in <module>
    exec(__fh.read())
  File "<string>", line 1, in <module>
  File "/home/ars-lab/ws_anjiejdroid_app/src/world_canvas/world_canvas_server/src/world_canvas_server/annotations_server.py", line 40, in <module>
    import warehouse_ros_mongo as wr
  File "/home/ars-lab/ws_android_app/devel/lib/python2.7/dist-packages/warehouse_ros_mongo/__init__.py", line 34, in <module>
    exec(__fh.read())
  File "<string>", line 1, in <module>
  File "/home/ars-lab/ws_android_app/src/warehouse_ros_mongo/src/warehouse_ros_mongo/message_collection.py", line 37, in <module>
    import pymongo as pm
ImportError: No module named pymongo
```

![image-20200709182454943](/home/ars-lab/zech_ws/NutNote/Typora data/image-20200709182454943.png)

### 解决:

[升级python2.7至python3.7](https://www.cnblogs.com/sheng518/p/11818562.html)

> 弃用:Python 2.7将在2020年1月1日结束它的生命。请升级你的Python，因为在那个日期之后Python  2.7将不会被维护。pip的未来版本将取消对Python 2.7的支持。关于pip中Python  2支持的更多细节，可以在https://pip.pypa.io/en/latest/development/relee-process/ #  python2 -support找到查看索引:http://mirrors.aliyun.com/pypi/simple/
>
> centos7版本默认安装的是python2.7。于是，升级开始了。
>
> 注意下，应为很多的依赖包基本命令什么的都是基于python2的，比如yum。所以旧版本不要删了，新旧可以共存。

```bash
$ sudo apt-get install python-pip
$ sudo pip install pymongo
```

![image-20200709235830707](/home/ars-lab/zech_ws/NutNote/Typora data/image-20200709235830707.png)

### 问题3:

```bash
File "/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_server/scripts/world_canvas_server", line 6, in <module>
    import world_canvas_server
  File "/home/ars-lab/ws_android_app/devel/lib/python2.7/dist-packages/world_canvas_server/__init__.py", line 34, in <module>
    exec(__fh.read())
  File "<string>", line 1, in <module>
  File "/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_server/src/world_canvas_server/annotations_server.py", line 42, in <module>
    from map_manager import *
  File "/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_server/src/world_canvas_server/map_manager.py", line 72, in <module>
    from world_canvas_msgs.msg import *
ImportError: No module named world_canvas_msgs.msg
```

### 解决:

```bash
$ cd ~/ws_android_app/src/
$ rosinstall world_canvas/ 
$ world_canvas/world_canvas.rosinstall
$ sudo apt install ros-melodic-yocs-msgs
$ sudo apt install ros-melodic-yocs-math-toolkit
$ cd ../
$ catkin_make
```

### 问题4:

```bash
qmake: could not exec '/usr/lib/x86_64-linux-gnu/qt4/bin/qmake': No such file or directory
CMake Error at /usr/share/cmake-3.10/Modules/FindQt4.cmake:1320 (message):
  Found unsuitable Qt version "" from NOTFOUND, this code requires Qt 4.x
Call Stack (most recent call first):
  world_canvas/world_canvas_tools/world_canvas_editor/CMakeLists.txt:17 (find_package)


-- Configuring incomplete, errors occurred!
See also "/home/ars-lab/ws_android_app/build/CMakeFiles/CMakeOutput.log".
See also "/home/ars-lab/ws_android_app/build/CMakeFiles/CMakeError.log".
Makefile:1020: recipe for target 'cmake_check_build_system' failed
make: *** [cmake_check_build_system] Error 1
Invoking "make cmake_check_build_system" failed
```

### **解决：**

```cpp
sudo apt-get install qt4-default 
https://blog.csdn.net/weixin_43159148/article/details/105439473
```

### 问题4:

![image-20200710003055470](/home/ars-lab/zech_ws/NutNote/Typora data/image-20200710003055470.png)

```bash
usr/include/boost/type_traits/detail/has_binary_operator.hp:50: Parse error at "BOOST_JOIN"
world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/build.make:67: recipe for target 'world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/moc_worlds_list.cxx' failed
make[2]: *** [world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/moc_worlds_list.cxx] Error 1
make[2]: *** Waiting for unfinished jobs....
usr/include/boost/type_traits/detail/has_binary_operator.hp:50: Parse error at "BOOST_JOIN"
world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/build.make:62: recipe for target 'world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/moc_annotations.cxx' failed
make[2]: *** [world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/moc_annotations.cxx] Error 1
usr/include/boost/type_traits/detail/has_binary_operator.hp:50: Parse error at "BOOST_JOIN"
world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/build.make:72: recipe for target 'world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/moc_editor_panel.cxx' failed
make[2]: *** [world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/moc_editor_panel.cxx] Error 1
CMakeFiles/Makefile2:4069: recipe for target 'world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/all' failed
make[1]: *** [world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[ 94%] Linking CXX executable /home/ars-lab/ws_android_app/devel/lib/world_canvas_client_examples/test_annotation_collection
[ 94%] Built target test_annotation_collection
Makefile:140: recipe for target 'all' failed
make: *** [all] Error 2
Invoking "make -j4 -l4" failed
```

### 解决:

```
编译代码时出现/usr/include/boost/type_traits/detail/has_binary_operator.hp:50: Parse error at "BOOST_JOIN"错误

临时解决方法：

修改/usr/include/boost/type_traits/detail/has_binary_operator.hpp文件

将namespace BOOST_JOIN(BOOST_TT_TRAIT_NAME,_impl) {

...

}

改为

#ifndef Q_MOC_RUN
namespace BOOST_JOIN(BOOST_TT_TRAIT_NAME,_impl) {
#endif

...

#ifndef Q_MOC_RUN
}
#endif
————————————————
版权声明：本文为CSDN博主「h321654」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/h321654/article/details/54582341
```

### 问题5:

```bash
In file included from /home/ars-lab/ws_android_app/src/world_canvas/world_canvas_tools/world_canvas_editor/src/worlds_list.cpp:21:0:
/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/map_loader.hpp: In static member function ‘static void MapLoader::load(const string&, nav_msgs::OccupancyGrid&)’:
/home/ars-lab/ws_android_app/src/world_canvas/world_canvas_tools/world_canvas_editor/include/world_canvas_editor/map_loader.hpp:125:110: error: cannot convert ‘bool’ to ‘MapMode’ for argument ‘8’ to ‘void map_server::loadMapFromFile(nav_msgs::GetMap::Response*, const char*, double, bool, double, double, double*, MapMode)’
 dMapFromFile(&map_resp_, mapfname.c_str(), res, negate, occ_th, free_th, origin, trinary);
                                                                                         ^
world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/build.make:105: recipe for target 'world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/src/worlds_list.cpp.o' failed
make[2]: *** [world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/src/worlds_list.cpp.o] Error 1
CMakeFiles/Makefile2:4069: recipe for target 'world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/all' failed
make[1]: *** [world_canvas/world_canvas_tools/world_canvas_editor/CMakeFiles/world_canvas_editor.dir/all] Error 2
Makefile:140: recipe for target 'all' failed
make: *** [all] Error 2
Invoking "make -j4 -l4" failed
```

### 解决:

原因: 调用 map_server 的函数,参数类型不对

```bash
https://github.com/ros-planning/navigation/pull/513/commits/ed2f1830b3c72b18bfee0213ea0cd8026d0cf02b
Add legacy signature with MapMode as bool 
```

下载navigation 中的map_server包,修改后重新编译

```bash
$ git clone https://github.com/ros-planning/navigation.git
$ cp -r navigation/map_server/ ~/ws_android_app/src/map_server
$ gedit ~/ws_android_app/src/map_server/src/image_loader.cpp
```

增加:

```cpp
#include "ros/ros.h"
#include "ros/console.h"
```

在namespace map_server
{

与
void
loadMapFromFile(nav_msgs::GetMap::Response* resp,

之间添加以下:

![image-20200710142036524](/home/ars-lab/zech_ws/NutNote/Typora data/image-20200710142036524.png)

```cpp
void
loadMapFromFile(nav_msgs::GetMap::Response* resp,
                const char* fname, double res, bool negate,
                double occ_th, double free_th, double* origin)
{
  loadMapFromFile(resp,
                  fname, res, negate,
                  occ_th, free_th, origin,
                  TRINARY);
}

void
loadMapFromFile(nav_msgs::GetMap::Response* resp,
                const char* fname, double res, bool negate,
                double occ_th, double free_th, double* origin,
                bool trinary)
{
  ROS_WARN(
    "map_server::loadMapFromFile with trinary as bool has been deprecated.",
    "Please update your code to use MapMode enumerable."
  );
  MapMode mode = (trinary)?TRINARY:SCALE;
  loadMapFromFile(resp,
                  fname, res, negate,
                  occ_th, free_th, origin,
                  mode);
}
```

```bash
$ gedit ~/ws_android_app/src/map_server/include/map_server/image_loader.h
```

![image-20200710142411169](/home/ars-lab/zech_ws/NutNote/Typora data/image-20200710142411169.png)

```cpp
    MapMode mode);

/** Legacy signature see issue #474
 *
 * @param resp The map wil be written into here
 * @param fname The image file to read from
 * @param res The resolution of the map (gets stored in resp)
 * @param negate If true, then whiter pixels are occupied, and blacker
 *               pixels are free
 * @param occ_th Threshold above which pixels are occupied
 * @param free_th Threshold below which pixels are free
 * @param origin Triple specifying 2-D pose of lower-left corner of image
 * @param trinary If true, only outputs Occupied/Free/Unknown
 * @throws std::runtime_error If the image file can't be loaded
 * */
void loadMapFromFile(nav_msgs::GetMap::Response* resp,
                    const char* fname, double res, bool negate,
                    double occ_th, double free_th, double* origin,
                    bool trinary);

/** Legacy signature see issue #474
 * Defaults last argument to TRINARY.
 *
 * @param resp The map wil be written into here
 * @param fname The image file to read from
 * @param res The resolution of the map (gets stored in resp)
 * @param negate If true, then whiter pixels are occupied, and blacker
 *               pixels are free
 * @param occ_th Threshold above which pixels are occupied
 * @param free_th Threshold below which pixels are free
 * @param origin Triple specifying 2-D pose of lower-left corner of image
 * @param trinary If true, only outputs Occupied/Free/Unknown
 * @throws std::runtime_error If the image file can't be loaded
 * */
void loadMapFromFile(nav_msgs::GetMap::Response* resp,
                    const char* fname, double res, bool negate,
                    double occ_th, double free_th, double* origin);
```

重新编译成功