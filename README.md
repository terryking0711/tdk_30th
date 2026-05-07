## simulation

### pre-mapping
1. LiDAR fusion
    - LiDAR plugin in Gazebo [source](https://classic.gazebosim.org/tutorials?tut=ros_gzplugins)
    - not filter yet
    - ira_laser_tools: laserscan_multi_merger [source](https://github.com/nakai-omer/ira_laser_tools/tree/humble)
2. mapping
    - package: SLAM Toolbox
    - save map for slam_toolbox: `ros2 service call /slam_toolbox/serialize_map slam_toolbox_msgs/srv/SerializePoseGraph "{filename: 'slam_map_0'}"`
    - save map for nav_amcl: `ros2 service call /slam_toolbox/save_map slam_toolbox_msgs/srv/SaveMap "{name: {data: 'amcl_map_0'}}"`
    - save map for catographer: 
        1. 停止軌跡: `ros2 service call /finish_trajectory cartographer_ros_msgs/srv/FinishTrajectory "{trajectory_id: 0}"`
        2. 序列化地圖 (類似 slam_toolbox 的 serialize): `ros2 service call /write_state cartographer_ros_msgs/srv/WriteState "{filename: '/home/ted/tdk_slam_ws/src/tdk_slam_manager/maps/carto_map_0.pbstream'}"`
        3. 將地圖轉換成 nav2 可以使用的圖片: 
            ```
            ros2 run cartographer_ros cartographer_pbstream_to_ros_map \
                -pbstream_filename /home/ted/tdk_slam_ws/src/tdk_slam_manager/carto_map_0.pbstream \
                -map_filestem /home/ted/tdk_slam_ws/src/tdk_slam_manager/maps/carto_map_0 \
                -resolution 0.05
            ```
3. execute in terminal w/virtual maze&machine
   ```
   colcon build
   source /opt/ros/humble/setup.bash
   source install/setup.bash
   ros2 launch tdk_slam_manager maze_world_launch.py
   ros2 launch sim_spawn_launch.py
   ```
4. interact&visulization
   ```
   rivz2
   #if want to make machine move
   ros2 run teleop_twist_keyboard teleop_twist_keyboard
   ```

#### notice
- 用teleop玩車的時候小心翻車。
