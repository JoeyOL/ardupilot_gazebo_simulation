# ardupilot_gazebo_simulation
## 首先确保你的系统是Ubuntu 22.04并且装有ros2 humble版本和mavros，并建议将桌面等文件名改为英文(自行百度)
## Step 1: 安装Micro XRCE DDS Gen
  - 打开一个新的终端，输入以下指令
    ```console
    sudo apt install default-jre
    git clone --recurse-submodules https://github.com/ardupilot/Micro-XRCE-DDS-Gen.git
    cd Micro-XRCE-DDS-Gen
    ./gradlew assemble
    ```
  - 将目录添加到$PATH中
    ```console
    sudo gedit ~/.bashrc
    # 在最后一行添加以下命令，并保存退出
    export PATH=$PATH:~/Micro-XRCE-DDS-Gen/scripts
    ```
    在新终端中输入microxrceddsgen -version有输出说明已完成
## 设置ardupilot的SITL
  - 打开终端输入以下指令
    ```console
    cd Desktop && mkdir -p ros2_ws/src
    cd ros2_ws/src
    git clone https://github.com/JoeyOL/ardupilot_gazebo_simulation/
    ```
    在桌面打开ros2_ws/src并将ardupilot_gazebo_simulation中的所有文件夹粘贴到ros2_ws/src中，并删除ardupilot_gazebo_simulation，在ros2_w/src中打开终端输入以下指令
    ```console
    cd ardupilot
    ./Tools/environment_install/install-prereqs-ubuntu.sh -y
    . ~/.profile
    
    ```
    完成后在先在终端输入以下指令，运行正常后(编译完成)，按Ctrl+C杀死进程
    ```console
    cd ArduCopter
    sim_vehicle.py -w
    ```
    最后执行以下命令，若出现仿真界面即为成功
    ```console
    sim_vehicle.py --console --map
    ```
## 安装Gazebo Garden
  - 在终端输入以下指令
    ```console
    sudo apt-get update
    sudo apt-get install lsb-release wget gnupg
    sudo wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-      stable.list > /dev/null
    sudo apt-get update
    sudo apt-get install gz-garden
    ```
    在终端输入 gz sim 指令出现gazebo界面即为成功
  - 在~/.bashrc中添加GZ_VERSION
    ```console
    sudo gedit ~/.bashrc
    # 在最后一行添加以下命令，并保存退出
    export GZ_VERSION=garden
## 安装ros-humble-desktop
  - 在终端输入以下指令
    ```console
    sudo apt update
    sudo apt install ros-humble-desktop
    ```
  - 初始化rosdep并安装依赖
    ```console
    cd ~/Desktop/ros2_ws
    source /opt/ros/humble/setup.bash
    sudo apt update
    sudo rosdep init
    rosdep update
    rosdep install --from-paths src --ignore-src -r
    ```
## Build
  - Build in ardupilot_gazebo
    打开终端输入
    ```console
    sudo apt update
    sudo apt install libgz-sim7-dev rapidjson-dev
    cd ~/Deskardupilot_gazebo
    mkdir build && cd build
    cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
    make -j4
    ```
    编译完成后输入以下指令:
    ```console
    echo 'export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/Desktop/ros2_ws/src/ardupilot_gazebo/build:${GZ_SIM_SYSTEM_PLUGIN_PATH}' >> ~/.bashrc
    echo 'export GZ_SIM_RESOURCE_PATH=$HOME/Desktop/ros2_ws/src/ardupilot_gazebo/models:$HOME/Desktop/ros2_ws/src/ardupilot_gazebo/worlds:${GZ_SIM_RESOURCE_PATH}' >> ~/.bashrc
    source ~/.bashrc
    ```
    在终端输入下面指令出现gazebo界面即为完成:
    ```console
    gz sim -v4 -r iris_runway.sdf
    ```
    打开另一个终端输入下面指令将gazebo和ardupilotSITL连接:
    ```console
    sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console
    ```
    可以在终端输入mode guided, arm throttle, takeoff 30看无人机是否会起飞
    若各界面无报错即为成功
  - Build in ros2_ws
    在终端输入以下命令
    ```console
    cd ~Desktop/ros2_ws
    colcon build
    ```
    只要最后显示所以包都finished就行，有stderr输出不用管
  - Build in ros_gz
    ```console
    cd ~Desktop/ros2_ws/src/ros_gz
    colcon build
    ```
    输出所有包finished即可
  - 启动
    ```console
    source ~/Desktop/ros2_ws/install/setup.sh
    ros2 launch ardupilot_gz_bringup iris_runway.launch.py
    ```
    启动后正常应该有gazebo界面rivz界面，打开新终端输入ros2 topic list出现ap/话题等即为成功
## Final:
  - 将SITL_Models中的模型路径添加到$PATH中
    ```console
    sudo gedit ~/.bashrc
    # 将下面的命令添加到最后一行
    export GZ_SIM_RESOURCE_PATH=$GZ_SIM_RESOURCE_PATH:\
    $HOME/Desktop/ros2_ws/src/SITL_Models/Gazebo/models:\
    $HOME/Desktop/ros2_ws/src/SITL_Models/Gazebo/worlds

    


    
