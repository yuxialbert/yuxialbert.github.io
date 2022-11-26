## Amd-rocm

### 安装
```bash
echo 'ADD_EXTRA_GROUPS=1' | sudo tee -a /etc/adduser.conf
echo 'EXTRA_GROUPS=video' | sudo tee -a /etc/adduser.conf
echo 'EXTRA_GROUPS=render' | sudo tee -a /etc/adduser.conf
# amd 
wget https://repo.radeon.com/amdgpu-install/22.20/ubuntu/focal/amdgpu-install_22.20.50200-1_all.deb
sudo apt-get install ./amdgpu-install_22.20.50200-1_all.deb
sudo amdgpu-install --usecase=hiplibsdk,rocm --no-dkms
sudo usermod -a -G video $LOGNAME
sudo usermod -a -G render $LOGNAME
echo 'export PATH=$PATH:/opt/rocm/bin:/opt/rocm/profiler/bin:/opt/rocm/opencl/bin' | sudo tee -a /etc/profile.d/rocm.sh
# 测试
rocm-smi
sudo /opt/rocm/bin/rocminfo
sudo /opt/rocm/opencl/bin/clinfo
```
如果直接安装有问题，进行以下操作
```
# rocm-llvm
apt download rocm-llvm
ar x rocm-llvm_xxxx.xxxxx_amd64.deb
tar xf control.tar.xz
vim control
# 修改Depends
Depends: python3, libc6, libstdc++6|libstdc++8, libstdc++-5-dev|libstdc++-7-dev|libstdc++-10-dev, libgcc-5-dev|libgcc-7-dev|libgcc-10-dev, rocm-core
# 打包
tar c postinst prerm control | xz -c > control.tar.xz
ar rcs rocm-llvm.deb debian-binary control.tar.xz data.tar.xz
sudo apt install libstdc++-10-dev libgcc-10-dev rocm-core
sudo dpkg -i rocm-llvm.deb
# rocm-gdb
apt download rocm-gdb
ar x rocm-gdb_xxxx.deb
tar xf control.tar.xz
vim control
# 修改Depends中的libpython3.8为python3
# 打包
tar c control | xz -c > control.tar.xz
ar rcs rocm-gdb.deb debian-binary control.tar.xz data.tar.xz
sudo apt install libtinfo5 libncurses5 comgr
sudo apt install rocm-dbgapi
sudo dpkg -i rocm-gdb.deb
```
libpython3.8
```bash
进软件和更新——其他软件——添加下面软件源
deb https://ppa.launchpadcontent.net/deadsnakes/ppa/ubuntu jammy main
sudo apt upgrade
sudo apt update
sudo apt install libpython3.8
```

### 参考
https://www.bilibili.com/read/mobile?id=19344356
https://blog.csdn.net/qq_44948500/article/details/127346390

