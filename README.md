# IMU-RTK-ROS2-simulation
IMU-RTK-ROS2-simulation是基于 ROS2 Humble 的纯仿真 IMU+RTK 组合导航系统，实现从高保真传感器仿真→数据预处理→时空同步→EKF 松组合→可视化定量分析的全流程开发，无硬件依赖，核心为 EKF 扩展卡尔曼滤波松组合，解决 GPS 失锁、轨迹漂移等经典问题，成果可量化。
✨ 项目亮点
纯仿真实现，基于 WSL2+Ubuntu22.04/ROS2 Humble，无硬件依赖，一键运行；
模块化 ROS2 工程结构，4 个功能包拆分，符合工业级开发规范；
高保真 IMU/RTK 仿真：模拟高斯噪声、零偏慢漂、RTK 固定解 / 失锁等真实特性；
实现传感器时空同步（时间戳对齐≤10ms）、WGS84→ENU 坐标高精度转换；
EKF 松组合落地：基于 robot_localization 配置 + Eigen3 理解底层原理；
鲁棒性优化：GPS 失锁 10s 内纯 IMU 积分漂移 < 2m，正常场景融合定位 RMSE<0.5m；
完整的可视化 + 定量分析：RVIZ2 实时轨迹展示、Matplotlib 量化误差分析。
🛠 技术栈
操作系统：WSL2 + Ubuntu 22.04
核心框架：ROS2 Humble
融合算法：EKF 扩展卡尔曼滤波（松组合）
关键库：Eigen3、GeographicLib、message_filters、tf2_ros
可视化 / 分析：RVIZ2、RQT、Matplotlib、Pandas
工具：Rosbag2、tf2_tools、rqt_reconfigure
📂 工程结构
plaintext
imu_rtk_ws/
└── src/
    ├── imu_rtk_sim/        # 高保真 IMU+RTK 仿真节点，生成带噪声的传感器数据
    ├── imu_rtk_preproc/    # 数据预处理：野值剔除、时空同步、WGS84→ENU 转换、TF 发布
    ├── imu_rtk_fusion/     # EKF 融合：robot_localization 配置、一键启动 launch、参数文件
    └── imu_rtk_visual/     # 可视化配置：RVIZ2 界面、定量分析脚本（误差计算 / RMSE）
├── README.md               # 项目说明书
└── doc/                    # 成果文档：轨迹对比图、RVIZ 可视化截图（可选）
🌐 环境搭建 & 快速运行
1. 安装基础依赖与核心库（WSL2/Ubuntu22.04）
bash
运行
# 安装ROS2 Humble基础功能包
sudo apt update && sudo apt install -y ros-humble-robot-localization ros-humble-message-filters ros-humble-tf2-tools ros-humble-rqt-reconfigure ros-humble-rosbag2-basic

# 安装核心依赖库
sudo apt install -y ros-humble-geographic-msgs libeigen3-dev python3-pip

# 安装Python数据分析库
pip3 install --no-cache-dir matplotlib pandas geographiclib
2. 克隆仓库 & 编译
bash
运行
# 克隆GitHub仓库（替换为你的实际仓库地址）
git clone https://github.com/你的用户名/imu-rtk-ros2-simulation.git ~/imu_rtk_ws/src

# 进入工作空间编译指定功能包
cd ~/imu_rtk_ws
colcon build --packages-select imu_rtk_sim imu_rtk_preproc imu_rtk_fusion imu_rtk_visual

# 加载ROS2环境变量
source install/setup.bash
3. 一键启动全系统
bash
运行
# 仿真+预处理+EKF融合+RVIZ2可视化 一键启动
ros2 launch imu_rtk_fusion full_system.launch.py
📈 项目成果
1. 轨迹对比与定量指标
RTK 原始轨迹 + IMU 纯积分轨迹 + EKF 融合轨迹 三轨迹叠加展示，融合轨迹无明显漂移，跟随性优异；
正常场景：EKF 融合定位 RMSE ＜ 0.5m；
GPS 失锁场景：纯 IMU 积分 10s 内漂移 ＜ 2m，鲁棒性良好。
2. 可视化展示
左侧面板：IMU/RTK 原始传感器数据、滤波后数据实时数值 + 曲线显示；
右侧面板：三轨迹叠加可视化 + TF 坐标系树实时展示 + 定位误差实时统计。
📝 核心功能流程
plaintext
高保真IMU/RTK仿真数据生成 → 数据预处理（野值剔除/滤波）→ 时空同步（时间戳对齐+TF坐标系构建）→ WGS84→ENU坐标转换 → EKF松组合（IMU预测+RTK观测更新）→ 可视化展示+定量误差分析 → GPS失锁鲁棒性补偿
📄 许可证
本项目采用 MIT License 开源协议，详情请查看项目根目录下的 LICENSE 文件。
