# livox 加到 阿克曼模型小车上

## racecar.urdf.xacro 调用 livox

`<!-- include livox -->`

  `<xacro:include filename="$(find livox_laser_simulation)/urdf/livox_to_car.xacro" />`

## livox_to_car.xacro 中将livox 加入到 car

文件最前面加上：

  `<xacro:include filename="$(find racecar_description)/urdf/sensors/lidar.xacro" />`

进行本文件最后几行的如下修改：

  `<xacro:Livox_AVIA name="livox"/>`

  `<!-- <xacro:include filename="$(find livox_laser_simulation)/urdf/standardrobots_oasis300.xacro"/> -->`

  `<joint name="livox_laser" type="fixed">`

​    `<parent link="laser_link"/>`

​    `<child link="livox_base"/>`

​    `<origin xyz="0.0 0 0.05" rpy="0 0 0" />`

  `</joint>`

  `<!-- <xacro:link_oasis name="oasis"/> -->`

## 修改源码 src/livox_laser_simulation/src/livox_points_plugin.cpp

注释文件最后的几行，如下：

​    `// tfBroadcaster->sendTransform(`

​    `//     tf::StampedTransform(tf, ros::Time::now(), raySensor->ParentName(), raySensor->Name()));`

因为他会发布livox到世界坐标系的变化，会污染livox到局部参考坐标系的变换

## car 相关的文件 去掉了命名空间 修改记录

/home/bqc/catkin_mp_ws/src/ackerman_simulation/racecar_description/scripts/servo_commands.py

/home/bqc/catkin_mp_ws/src/ackerman_simulation/bringup/launch/gazebo/racecar.launch

主要去掉了robot joint state publish 的命名空间、控制器的命名空间 ns

/home/bqc/catkin_mp_ws/src/ackerman_simulation/racecar_description/urdf/racecar.urdf.xacro

/home/bqc/catkin_mp_ws/src/ackerman_simulation/bringup/config/ctrl.yaml
