# A Perfect Pipeline for Building Enviroment about GPU

## Download some necessary file:
	NVIDIA Driver: https://www.geforce.cn/drivers
		lspci | grep -i nvidia
		lspci | grep VGA : 查看显卡型号，下载适应的驱动；

	cuda toolkit : https://www.geforce.cn/drivers
		下载和显卡对应的cuda版本；
	
	cudnn:  https://developer.nvidia.com/rdp/cudnn-download

## Close BIOS secure root
	necessary

## 禁用 nouveau
	sudo vim /etc/modprobe.d/blacklist.conf
	
		blacklist nouveau
		blacklist lbm-nouveau
		options nouveau modeset=0
		alias nouveau off
		alias lbm-nouveau off

	echo options nouveau modeset=0
	sudo update-initramfs -u
	
	sudo reboot
	
	运行 lsmod | grep nouveau 如果没有输出内容，则禁用成功;

## 关闭Linux图形界面
	For Ubuntu 18.04:
		sudo systemctl set-default multi-user.target
		sudo reboot

## Install driver
	1. 删掉之前的驱动： sudo apt-get remove --purge nvidia*
	2. 更新安装一些必要的库： sudo apt-get update 
				  sudo apt-get install dkms build-essential linux-headers-generic
	3. 安装驱动： sudo chmod u+x NVIDIA-Linux-x86_64***.run
		      sudo ./NVIDIA-Linux-***.run -no-opengl-files
	* 如果 -no-opengl-files 报错，可尝试直接运行run文件

## Install cuda
	1. 补充一些库： sudo apt-get install freeglut3-dev libx11-dev libxmu-dev libxi-dev libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev
	2. md5检测： md5sum cuda***.run
	3. sudo sh cuda***.run -no-opengl-files
	* 同上，如果 -no-opengl-files 报错，可忽略此参数尝试
	4. 添加环境变量：
		vim ~/.bashrc
			export CUDA_HOME=/usr/local/cuda
			export PATH=$PATH:$CUDA_HOME/bin
			export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
	5. 检测命令：
		cat /proc/driver/nvidia/version
		nvcc -V

## Install cudnn
	直接解压安装：
	 sudo dpkg -i libcudnn7_7.4.2.24-1+cuda10.0_amd64.deb
	 sudo dpkg -i libcudnn7-dev_7.4.2.24-1+cuda10.0_amd64.deb
	 sudo dpkg -i libcudnn7-doc_7.4.2.24-1+cuda10.0_amd64.deb

## Install Anaconda 
	比较简单，直接运行sh文件，记得添加环境变量即可；
	vim ~/.bashrc
		export PATH="/home/xxx/anaconda3/bin:$PATH"
	source ~/.bashrc

## Install Torch
	官网根据自己配置选择命令安装即可：https://pytorch.org/get-started/locally/

## Tips
	* 有些显卡型号比较新，可能一开始运行 lspci | grep VGA 不能得到准确的显卡名称，此时可能需要根据硬件判断显卡型号；
	  或者尝试命令 sudo update-pciids 更新显卡id，即可看到显卡名称；
