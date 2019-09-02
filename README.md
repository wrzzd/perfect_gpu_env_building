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

## 禁用 nouveau:
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


