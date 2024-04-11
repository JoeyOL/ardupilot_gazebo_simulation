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
