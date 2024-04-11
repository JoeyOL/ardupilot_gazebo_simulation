# ardupilot_gazebo_simulation
## 首先确保你的系统是Ubuntu 22.04并且装有ros2 humble版本，并建议将桌面等文件名改为英文(自行百度)
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
  - 在终端输入以下命令
    ```console
    cd ~Desktop/ros2_ws
    colcon build
    ```
    只要最后显示所以包都finished就行，有stderr输出不用管
  - 启动
    ```console
    source ~/Desktop/ros2_ws/install/setup.sh
    ros2 launch ardupilot_gz_bringup iris_runway.launch.py
    ```
    启动后正常应该有gazebo界面rivz界面，打开新终端输入ros2 topic list出现ap/话题等即为成功
  - Build in ros_gz
    ```console
    cd ~Desktop/ros2_ws/src/ros_gz
    colcon build
    ```
