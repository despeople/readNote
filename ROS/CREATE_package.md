```ros
catkin_creat_pkg ssr_pkg rospy roscpp std_msgs

创建在/catkin_ws/src下的Package软件包

catkin_creat_pkg <包名> <依赖项列表>

可以通过
roscd  <依赖项> 
roscd roscpp
访问依赖项在系统中的位置
```

![image-20250618164344307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250618164344307.png)



rosrun ssr_pkg chao_node 运行节点

rostopic list 查看活跃的节点

rostopic echo /chao_node_send 查看该节点的data信息

rostopic hz /chao_node_send 查看发送频率

![image-20250618171737803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250618171737803.png)

