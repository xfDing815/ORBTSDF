# 创建工作空间
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin init
catkin config --extend /opt/ros/noetic
catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release
catkin config --merge-devel

#克隆代码
cd ~/catkin_ws/src
git clone git@github.com:xfDing815/ORBTSDF.git

#编译
catkin build orb_slam_2_ros

#报错1 Panglion
ffmpeg.cpp:47:41: error: ‘AV_PIX_FMT_VDPAU_MPEG4’ was not declared in this scope
https://blog.csdn.net/Robert_Q/article/details/121690089

#这些缺损的文件可以网上下载，或者直接拷贝 
glog_catkin cv_bridge_new eigen_catkin eigen_checks gflags_catkin glog_catkin minkindr minkindr_ros

修改run_orb_slam_2_d435i.launch中ORBvoc.txt、RealSense_D435i.yaml的位置

在rgbd_dataset_d435i.launch文件中第10行添加如下代码（降噪）
  <!-- Run a VoxelGrid filter to clean NaNs and downsample the data -->
  <node pkg="nodelet" type="nodelet" name="point_cloud_pass_through" args="load pcl/PassThrough pcl_manager" output="screen">
    <remap from="~input" to="/camera/depth/color/points" />
    <rosparam>
      filter_field_name: z
      filter_limit_min: 0.01
      filter_limit_max: 1.0
      filter_limit_negative: False
    </rosparam>
  </node>

#运行
roslaunch orb_slam_2_ros run_orb_slam_2_d435i.launch
cd realsense_ws roslaunch realsense2_camera rs_camera.launch enable_pointcloud:=true align_depth:=true
roslaunch voxblox_ros rgbd_dataset_d435i.launch
rviz
save:rosservice call /voxblox_node/generate_mesh "{}"
