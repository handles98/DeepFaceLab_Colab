# Installation for Ubuntu 16.04

An installation script has been created to automatically install all of the required dependencies for Ubuntu 16.04. Clone the repository and run ``ubuntu16.04-cuda9-installer.sh`` from the root directory of DeepFaceLab_Linux. 

# Installation for Ubuntu 18.04

#### Add NVIDIA package repositories
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.0.130-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804_10.0.130-1_amd64.deb
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo apt-get update
```

#### Install NVIDIA Video Driver
```bash
sudo apt-get install --no-install-recommends nvidia-driver-418
```
**Reboot your system.**

## Easy Method; No DLIB CUDA support

### Install Anaconda3
Download the installer [https://www.anaconda.com/distribution/#linux](https://www.anaconda.com/distribution/#linux). Install Anaconda3 and choose the defaults. You will also need to add conda to your path so you can complete the final steps.

### Install DeepFaceLab

```bash
conda create -y -n deepfacelab python=3.6.6 cudatoolkit=9.0 cudnn=7.3.1
conda activate deepfacelab
git clone https://github.com/lbfs/DeepFaceLab_Linux.git
cd DeepFaceLab_Linux
python -m pip install -r requirements-cuda.txt
```

## Harder Alternate Method; Allows for DLIB CUDA support

For this method, we will create an Ubuntu 16.04 container on your system. In order to do this, we will need to install and configure LXD. 
```bash
sudo snap install lxd
sudo lxd init
sudo adduser "$USER" lxd
```
**Reboot or logout so the new group membership can take effect**

After you have finished installing and configuring lxd to your needs. We will now need to create the container. 

```bash
echo "root:$UID:1" | sudo tee -a /etc/subuid /etc/subgid #Only run once and never again!
wget https://blog.simos.info/wp-content/uploads/2018/06/lxdguiprofile.txt #Thanks to Simos Xenitellis for his GUI LXC profile!
lxc profile create gui
cat lxdguiprofile.txt | lxc profile edit gui
lxc launch --profile default --profile gui ubuntu:16.04 deepfacelab
# Wait 30s so the environment can fully setup without issue.
# Logging in before the inital setup is done can cause problems.
# The next command will fix the DeepFaceLab GUI to allow it to show up correctly.
lxc exec deepfacelab -- sh -c "echo 'export QT_X11_NO_MITSHM=1' >> /home/ubuntu/.bashrc"
```

You can now access your container at any time with the following command
```bash
lxc exec deepfacelab -- su ubuntu
```

While in the container, change to your home directory with ``cd ~\`` and then run the installation instructions for Ubuntu 16.04 and you will have created an identical environment.

**WARNING: Make sure you install the same video driver in the container as installed in the host!**
