# 树莓派刷入流程

> 资源来源尽量使用中科大的镜像
> 
- 刷入系统
- 下载安装Imager [Index of /raspberry-pi-os-images/imager/ (ustc.edu.cn)](https://mirrors.ustc.edu.cn/raspberry-pi-os-images/imager/)
- 下载镜像 [Index of /raspberry-pi-os-images/raspios_lite_arm64/images/ (ustc.edu.cn)](https://mirrors.ustc.edu.cn/raspberry-pi-os-images/raspios_lite_arm64/images/)
- 清空SD卡并选择镜像刷入
    
    > 新版本imager支持在刷入前对系统进行一定的配置, 确认好再开始刷入
    > 
- 插入SD卡, 插电开机
- 默认通过域名rpi.local连接
- 更新APT Source, 并更新到最新状态
    
    ```bash
    sudo sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
    sudo sed -i 's|security.debian.org/debian-security|mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list
    sudo sed -i 's|//archive.raspberrypi.org|//mirrors.ustc.edu.cn/archive.raspberrypi.org|g' /etc/apt/sources.list.d/raspi.list
    sudo apt update && sudo apt upgrade -y
    # 安装常用软件
    sudo apt install -y curl vim wget git
    ```
    
## [Docker](./software/Docker.md)

## [Nodejs](./software/NodeJS.md)

## [Others](./software/Others.md)
