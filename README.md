steps for setting GPU for DeepLearning

Ubuntu 20.04, RTX A6000
Ubuntu 20.04, A40
Ubuntu 20.04, A30

1. Install NVIDIA display driver - 535.34
	Download .run file from nvidia drivers site and install it
	
	chmod +x some-app.run
	sudo ./some-app.run

2. Install CUDA - 11.7
	https://developer.nvidia.com/cuda-downloads
	
	set path in ~/.bashrc
export PATH=$PATH:/usr/local/cuda/bin
export CUDADIR=/usr/local/cuda
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64

	source ~/.bashrcs

3. Install cuDNN - 8.9
	https://developer.nvidia.com/rdp/cudnn-download
	
	sudo dpkg -i cudnn.deb

Done

reference: https://medium.com/geekculture/deep-learning-gpu-setup-from-scratch-75f730c49c01

---

Install Docker - https://docs.docker.com/engine/install/ubuntu/
Install nvidia container toolkit - # https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#setting-up-nvidia-container-toolkit
								 - https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#setting-up-nvidia-container-toolkit

keep --gpus all in docker run command

add docker in sudoers group
$sudo groupadd docker
$sudo usermod -aG docker $USER

---

solve /localhome issue

There are two steps to bind mount a home directory to a different location:

the bind mount: create the mount point and run the mount command:
$ sudo mkdir -p /home/$USER
$ sudo mount --bind <original-home-location> /home/$USER
Example $  sudo mount --bind /localhome/local-dmukeshbhai/ /home/local-dmukeshbhai/

edit /etc/passwd: backup passwd and edit the home location for your user:
$ cp /etc/passwd passwd.backup
$ # sudo edit /etc/passwd with your favourite editor
$ cat /etc/passwd | grep $USER
  ubuntu:x:1000:1000:ubuntu,,,:/home/ubuntu:/bin/bash
  
The following awk command can be used to edit /etc/passwd (change OLD_HOME to your old home directory):
$ awk -vold=$"OLD_HOME" -vnew=$"/home/$USER" -F: ' BEGIN {OFS = ":"} \
  {sub(old,new,$6);print}' /etc/passwd > passwd.new
$ sudo cp passwd.new /etc/passwd

Log out and back in again, and snap will work from the freshly mounted home location. If you run into difficulties, copy the backup passwd file to /etc/passwd.

---

add mocro k8s into sudoers group if getting permission denied error in enable command

---
