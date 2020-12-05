# CUDA Installation

CUDA is a parallel computing platform and programming model  invented by NVIDIA. It enables dramatic increases in computing  performance by harnessing the power of the graphics processing unit (GPU).



## Pre-installation Actions

- Verify the system has a CUDA-capable GPU.
- Verify the system is running a supported version of Linux.
- Verify the system has gcc installed.
- Verify the system has the correct kernel headers and development packages installed.



We should make sure that distribution versions can support our CUDA Toolkit version. For my case, CUDA 10.0 is supported by 

```shell
$ uname -r
```

4.15.0-65-generic

```shell
$ gcc --version
```

gcc (Ubuntu 5.4.0-6ubuntu1~16.04.11) 5.4.0 20160609

```Shell
$ lspci | grep -i nvidia
```

01:00.0 VGA compatible controller: NVIDIA Corporation GP106M [GeForce GTX 1060 Mobile] (rev a1)
01:00.1 Audio device: NVIDIA Corporation GP106 High Definition Audio Controller (rev a1)

```shell
$ uname -m && cat /etc/*release
```

x86_64
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.6 LTS"



The kernel headers and development packages for the currently running kernel can be installed with:                               

```shell
$ sudo apt-get install linux-headers-$(uname -r)
```



## Installation

*These instructions are for installing CUDA through the repository instead of the .deb installation.*

The following lines you can copy and paste to a terminal window.  Press `Ctrl`+`Alt`+`T` to open a terminal window.

Remove any CUDA PPAs that may be setup and also remove the `nvidia-cuda-toolkit` if installed:

```shell
$ sudo rm /etc/apt/sources.list.d/cuda*
$ sudo apt remove --autoremove nvidia-cuda-toolkit
```

Recommended to also remove all NVIDIA drivers before installing new drivers:

```shell
$ sudo apt remove --autoremove nvidia-*
```

Then update the system:

```shell
$ sudo apt update
```

Recently, I just found out that the CUDA installation works with the `graphics-drivers ppa` so if you don't have it added, add it now:

```shell
$ sudo add-apt-repository ppa:graphics-drivers/ppa
```

Install the key:

```shell
$ sudo apt-key adv --fetch-keys  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
```

Add the repos:

```shell
$ sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/cuda.list'

$ sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'
```

Update the system again:

```shell
$ sudo apt update
```

Install CUDA 10.1:

```shell
$ sudo apt install cuda-10-0
```

It should be installing the NVIDIA 418.40 drivers with it as those are what are listed in the repo.  See:  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/

Install libcudnn7 7.5.1:

```shell
$ sudo apt install libcudnn7
```



## Runfile Installation

1. Download CUDA:

   https://developer.nvidia.com/cuda-downloads

2. Remove all related packages befor installing new drivers:

   ```shell
   $ sudo apt-get purge nvidia-*
   ```

   Then update the system:

   ```shell
   $ sudo apt update
   ```

3. Disabling Nouveau

   The Nouveau drivers are loaded if the following command prints anything:

   ```shell
   $ lsmod | grep nouveau
   ```

   - Create a file at `/etc/modprobe.d/blacklist-nouveau.conf` with the following contents:

     ```shell
     blacklist nouveau
     options nouveau modeset=0
     ```

   - Regenerate the kernel initramfs:                                     

     ```shell
     $ sudo update-initramfs -u
     ```

4. Logout from your GUI into text terminal

   `ctrl` + `alt` + `F1`

   - Close  X display manager (LightDM)

     ```shell
     $ sudo service lightdm stop
     ```

   - Run the installer and follow the on-screen prompts:                                  					

     ```shell
     $ sudo sh cuda_<version>_linux.run
     ```

   - Restart LightDM

     ```shell
     $ sudo service lightdm start
     ```



## Post-installation Actions

1. Environment Setup

   - Open your .bashrc file: 

     ```shell
     $ atom ~/.bashrc
     ```

   - Append  the following two lines to the end of the file:

     ```shell
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
     export CUDA_HOME=/usr/local/cuda
     ```
   
   - Add the following lines to your `~/.profile` file for CUDA 10.0
   
     ```
     # set PATH for cuda 10.0 installation
     if [ -d "/usr/local/cuda-10.0/bin/" ]; then
         export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
         export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
     fi
     ```
   
     

## Verify CUDA installation

It is important to verify that the CUDA toolkit can find and communicate correctly with the CUDA-capable
hardware.



```shell
$ nvcc --version
```

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130



```shell
$ nvidia-smi
```

Thu Oct 17 11:51:15 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.87.01    Driver Version: 418.87.01    CUDA Version: 10.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1060    Off  | 00000000:01:00.0  On |                  N/A |
| N/A   53C    P5     7W /  N/A |    979MiB /  6078MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1166      G   /usr/lib/xorg/Xorg                           526MiB |
|    0      2107      G   compiz                                       303MiB |
|    0      2567      G   ...-token=28B57599FCC0EBBB36177F0519464181   110MiB |
|    0      4200      G   ...uest-channel-token=12850143586756335369    35MiB |
+-----------------------------------------------------------------------------+