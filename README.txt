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

FindFFMPEG.cmake中63,64行
        sizeof(AVFormatContext::max_analyze_duration);
      }" HAVE_FFMPEG_MAX_ANALYZE_DURATION

ffmpeg.cpp中第37行 namespace pangolin上面加上
#define CODEC_FLAG_GLOBAL_HEADER AV_CODEC_FLAG_GLOBAL_HEADER

第78,79行
#ifdef FF_API_XVMC
    TEST_PIX_FMT_RETURN(XVMC_MPEG2_MC);
    TEST_PIX_FMT_RETURN(XVMC_MPEG2_IDCT);
#endif

101-105行
#ifdef FF_API_VDPAU
    TEST_PIX_FMT_RETURN(VDPAU_H264);
    TEST_PIX_FMT_RETURN(VDPAU_MPEG1);
    TEST_PIX_FMT_RETURN(VDPAU_MPEG2);
    TEST_PIX_FMT_RETURN(VDPAU_WMV3);
    TEST_PIX_FMT_RETURN(VDPAU_VC1);
#endif

127行
#ifdef FF_API_VDPAU
    TEST_PIX_FMT_RETURN(VDPAU_MPEG4);
#endif

#这些缺损的文件可以网上下载，或者直接拷贝 
glog_catkin cv_bridge_new eigen_catkin eigen_checks gflags_catkin glog_catkin minkindr minkindr_ros
