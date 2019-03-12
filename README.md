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

## Easy Method

### Install Anaconda3
Download the installer [https://www.anaconda.com/distribution/#linux](https://www.anaconda.com/distribution/#linux). Install Anaconda3 and choose the defaults. You will also need to add conda to your path so you can complete the final steps.

### Install DeepFaceLab

```bash
conda create -y -n deepfacelab python==3.6.6 cudatoolkit==9.0 cudnn
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
```

After you have finished installing and configuring lxd to your needs. We will now need to create the container. 

```bash
lxc launch ubuntu:16.04 deepfacelab
lxc stop deepfacelab
lxc config device add deepfacelab gpu gpu
lxc start deepfacelab
```

You can now access your container at any time with the following command
```bash
lxc exec deepfacelab -- su ubuntu
```

While in the container, run the installation instructions for Ubuntu 16.04 and you will have created an identical environment.
**WARNING: Make sure you install the same video driver in the container as installed in the host!**