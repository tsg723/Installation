## 安装exfat格式硬盘支持

cn99更新软件源

```shell
sudo apt-get install exfat-utils
```



## 安装显卡驱动

* ```shell
   sudo gedit /etc/modprobe.d/blacklist.conf
   ```

   打开后添加

   ```shell
   blacklist nouveau
   blacklist lbm-nouveau
   options nouveau modeset=0
   alias nouveau off
   alias lbm-nouveau off
   ```

* ```shell
   echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf build the new kernel by:
   sudo update-initramfs -u
   reboot
   ```

* ```shell
   sudo apt-get install gcc g++ make
   ```

* ```shell
   sudo sh NVIDIA-Linux-x86_64-440.31.run        # 安装包修改为对应版本
   ```



## 安装cuda

* ```shell
   sudo sh cuda_10.0.130_410.48_linux.run        # 安装包修改为对应版本
   ```

   如果出现安装显卡驱动的选项，不勾选该项

* ```shell
   sudo gedit ~/.bashrc
   ```

   打开后添加

   ```shell
   export PATH=$PATH:/usr/local/cuda/bin
   export LD_LIBRARY_PATH=/usr/local/cuda/bin/lib64:$LD_LIBRARY_PATH   
   ```



## 安装cudnn

```shell
tar -xzvf cudnn-10.0-linux-x64-v7.6.3.30.tgz      # 安装包修改为对应版本
sudo cp cuda/include/cudnn.h /usr/local/cuda-10.0/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-10.0/lib64
sudo chmod a+r /usr/local/cuda-10.0/include/cudnn.h /usr/local/cuda-10.0/lib64/libcudnn*

sudo dpkg -i libcudnn7_7.6.3.30-1+cuda10.0_amd64.deb
sudo dpkg -i libcudnn7-dev_7.6.3.30-1+cuda10.0_amd64.deb
sudo dpkg -i libcudnn7-doc_7.6.3.30-1+cuda10.0_amd64.deb
```



## 安装常用软件

openssh-server，pycharm，anaconda，anydesk，chrome，搜狗输入法。。。

---

**设置pycharm快捷方式**

```shell
sudo  gedit  /usr/share/applications/Pycharm.desktop
```

打开后输入

```shell
[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec=sh /home/lcc/pycharm-community-2019.2.4/bin/pycharm.sh       # 路径修改为安装路径
Icon=/home/lcc/pycharm-community-2019.2.4/bin/pycharm.png
Terminal=pycharm
Categories=Pycharm;
```

---

**anaconda设置清华源**

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --set show_channel_urls yes
```

---

**卸载火狐浏览器**

```shell
sudo apt-get purge firefox firefox-locale-en firefox-locale-zh-hans
```

---

**挂载硬盘**

```shell
sudo fdisk -l
sudo umount /dev/sdb
sudo mount /dev/sdb /media/10T
```

**自动挂载**

```shell
sudo blkid       # 查UUID
```

```shell
sudo gedit /etc/fstab
```

打开后添加查到的UUID

```shell
UUID /media/10T ext4 defaults 0 0
```



## 安装使用多版本cuda，cudnn

新的cuda安装完成后，新的cudnn装在对应的cuda文件夹中

* 方法一：修改软链接

  环境变量配置为

  ```shell
  export PATH=$PATH:/usr/local/cuda/bin
  export LD_LIBRARY_PATH=/usr/local/cuda/bin/lib64:$LD_LIBRARY_PATH
  ```

  每次切换cuda版本，只需要修改软链接

  ```shell
  sudo rm -rf /usr/local/cuda
  sudo ln -s /usr/local/cuda-8.0 /usr/local/cuda
  ```

* 方法二：修改环境变量（对于无法修改软链接的非root用户）

  ```shell
  export PATH=$PATH:/usr/local/cuda-8.0/bin
  export LD_LIBRARY_PATH=/usr/local/cuda-8.0/bin/lib64:$LD_LIBRARY_PATH
  ```

  
