# IMU-RTK-ROS2-simulation
IMU-RTK-ROS2-simulation 是基于 ROS2 Humble 的纯仿真 IMU+RTK 组合导航系统，实现从高保真传感器仿真→数据预处理→时空同步→EKF 松组合→可视化定量分析的全流程开发，无硬件依赖，核心为 EKF 扩展卡尔曼滤波松组合，解决 GPS 失锁、轨迹漂移等经典问题，成果可量化。


## ✨ 项目亮点
- 纯仿真实现，基于 WSL2+Ubuntu22.04/ROS2 Humble，无硬件依赖，一键运行；
- 模块化 ROS2 工程结构，4 个功能包拆分，符合工业级开发规范；
- 高保真 IMU/RTK 仿真：模拟高斯噪声、零偏慢漂、RTK 固定解/失锁等真实特性；
- 实现传感器时空同步（时间戳对齐≤10ms）、WGS84→ENU 坐标高精度转换；
- EKF 松组合落地：基于 robot_localization 配置 + Eigen3 理解底层原理；
- 鲁棒性优化：GPS 失锁 10s 内纯 IMU 积分漂移 < 2m，正常场景融合定位 RMSE<0.5m；
- 完整的可视化 + 定量分析：RVIZ2 实时轨迹展示、Matplotlib 量化误差分析。


## 🛠 技术栈
- 操作系统：WSL2 + Ubuntu 22.04
- 核心框架：ROS2 Humble
- 融合算法：EKF 扩展卡尔曼滤波（松组合）
- 关键库：Eigen3、GeographicLib、message_filters、tf2_ros
- 可视化/分析：RVIZ2、RQT、Matplotlib、Pandas
- 工具：Rosbag2、tf2_tools、rqt_reconfigure


## 📂 工程结构
```plaintext 
imu_rtk_ws/
├── src/
│   ├── imu_rtk_sim/        # 高保真IMU+RTK仿真节点，生成带噪声的传感器数据
│   ├── imu_rtk_preproc/    # 数据预处理：野值剔除、时空同步、WGS84→ENU转换、TF发布
│   ├── imu_rtk_fusion/     # EKF融合：robot_localization配置、一键启动launch、参数文件
│   └── imu_rtk_visual/     # 可视化配置：RVIZ2界面、定量分析脚本（误差计算/RMSE）
├── README.md               # 项目说明书
└── doc/                    # 成果文档：轨迹对比图、RVIZ可视化截图（可选）
```

