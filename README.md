# Jetson Nano Guide
A step-by-step guide to set up from scratch and use an NVIDIA Jetson Nano device for real-time machine learning projects.
This repository contains useful commands, advices and resources for quickly setting up and working with an NVIDIA Jetson Nano. Most of these can also be used for other NVIDIA Jetson series devices. 

Even though there are official NVIDIA tutorials online, they have some problems, as they contain some outdated configurations, while they focus on running the NVIDIA machine learning inference examples, and not your own machine learning projects with custom code and environment settings. 

<br><br>

# Setup 
The following are useful guides for setting up the device:
- [Get started guide](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)
- [Developer kit PDF](https://developer.nvidia.com/embedded/downloads#?search=Jetson%20Nano%20Developer%20Kit%20User%20Guide)

# Power supply 
A 5V⎓2A source is recommended. Common Micro USB connection with a computer should also work. For this kind of power supply, make sure you remove the J48 jumper from the device. You do not need to power the device just yet. 

# MicroSD card
A MicroSD card is typically included with NVIDIA Jetson devices. This is used as a boot device and for main storage. You can also use your own card but it is recommended that it is at least 32 GB UHS-1. Follow [these](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write) steps to set up the MicroSD card with the Ubuntu image, depending on your OS.  

# First Boot
The original guide can be followed for first boot. See instructions [here](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#setup). I recommend "headless mode" to access the device as it is easier if you are already using a computer, and does not require external hardware like keyboard. After following the instructions, you should be able to log in the device with your personal account 

# Ethernet, Wi-Fi Access and OS update
The easiest way to set up internet access is by connecting an ethernet cable from you home router to the device. Once you do that you can also set up wireless connection if you have a network card or a Wi-Fi Bluetooth adapter. 

To test Internet connection, you can just ping Google and see if you receive packages back:

    ping 8.8.8.8

Check IP with:

    ifconfig wlan0

Once internet access is established, it is recommended that you update the OS version of your system, as well as included software. To do that:

    sudo apt-get update
    sudo apt-get upgrade
    sudo reboot now

To add Wi-Fi access with a Wi-Fi Bluetooth adapter, [this](https://learn.sparkfun.com/tutorials/adding-wifi-to-the-nvidia-jetson/all) is a nice guide to follow. I have tested it with an Edimax Wi-Fi adapter and can confirm that it works. 

Downloading and installing the drivers should allow your Wi-Fi Bluetooth adapter to connect to your Wi-Fi network. Note that if your NVIDIA Jetson device is accessed via "headless mode" with ssh, the computer and the Jetson must connect to the same Wi-Fi as different guides suggest. Let's try to connect to the Wi-Fi now.

Turn Wi-Fi on:

    nmcli r wifi on

Check the available WiFi networks: 

    nmcli d wifi list

Connect with network:
    
    sudo nmcli d wifi connect <SSID> password <PASSWORD>

Test connection:

    ping 8.8.8.8

In case of network problems, check the following things:

--> Distance to your router. If the Wi-Fi router is not very close to Jetson, the device may have frequent network interruptions. An easy way to solve this for development and testing is to use your smartphone Wi-Fi hotspot, placing it near your Jetson, and connecting both your PC and the Jetson to your Wi-Fi via your smartphone's hotspot.

--> If both wlan0 and wlan1 are connected to the Wi-Fi, there might be some internet connection problems. In this case, deactivate wlan1 and then reconnect to your network:

    sudo ifconfig wlan1 down
    sudo nmcli d wifi connect <SSID> password <PASSWORD>

--> If problems with Wi-Fi even though the SSID and password are correct, first do:

    sudo systemctl restart NetworkManager

# Memory

Jetson Nano includes 4 GB (64-bit LPDDR4) Memory. Recent releases of L4T/JetPack enable swap memory as part of the default distribution (see also this [guide](https://jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/) for more information). In the default distribution, the swap memory size is 2GB. At times this may not be enough if you are doing very large projects.

Examine memory information:

    zramctl

You will notice that there are four entries (one for each CPU of the Jetson Nano) /dev/zram0 – /dev/zram3. Each device has an allocated amount of swap memory associated with it, by default 494.6M, for a total of around 2GB. This is half the size of the main memory.

Show memory and swap size:
free -m

[This](https://www.youtube.com/watch?v=uvU8AXY1170&t=737s) is a useful tutorial to increase SWAP memory for large projects

<br><br>

# Machine Learning with Python

Each Jetson device has its own JetPack SDK version with accelerated libraries for deep learning, computer vision, graphics and more. However, each JetPack SDK version supports specific Python versions, which can limit the types of machine learning projects you can run in these devices. You can check the JetPack SDK version of your device to assess what Python version projects you can run. An indicative guide can be seen [here](https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048). For example, Jetson Nano supports Python 2.7 and Python 3.6 by default which are now considered deprecated by state-of-the-art deep learning projects. 

You can install newer Python versions, but there won't be native support for cuDNN libraries. This will result in slower inference. cuDNN accelerates widely used deep learning frameworks, including Caffe2, Chainer, Keras, MATLAB, MxNet, PaddlePaddle, PyTorch, and TensorFlow. Empirical benchmarks and experiences from the deep learning community suggest that cuDNN can provide a 2x to 10x speed-up over using CUDA alone.

To summarize, it is recommended to setup a newer Python version to take advantage of state-of-the-art deep learning libraries but you have to keep in mind the consequences this might have depending on the JetPack SDK version your device has.


# Python Setup

[This](https://www.youtube.com/watch?v=LSdXakt8nZ8) and [this](https://i7y.org/en/yolov8-on-jetson-nano/) guide show useful information for seting up newer Python version in Jetson devices. 

For example, for Python 3.8, install Python 3.8:

    sudo apt update
    sudo apt install -y python3.8 python3.8-venv python3.8-dev python3-pip \
    libopenmpi-dev libomp-dev libopenblas-dev libblas-dev libeigen3-dev libcublas-dev binutils

Create and activate Python 3.8 environment:

    python3.8 -m venv venv
    source venv/bin/activate

Install PyTorch using [this](https://i7y.org/en/yolov8-on-jetson-nano/) guide or the [wheel](https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048) by NVIDIA. 

Check if installed pytorch in your Python environment supports CUDA (make sure you have your Python environmnet with PyTorch installed activated). This will output True if CUDA is available:

    python3.8
    import torch
    print(torch.cuda.is_available())

Check if installed pytorch in your Python environment supports cuDNN (make sure you have your Python environmnet with PyTorch installed activated). This will output cuDNN version if cuDNN is available:

    python3.8
    import torch
    print(torch.backends.cudnn.version())


# Git 

In order to download your deep learning projects from GitHub, it is recommended that you setup your Git account. 

Configure git profile:

    git config --global user.name "Your Name"
    git config --global user.email "your.email@example.com"

Generate ssh key for authentication:

    ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

When asked where to store it, press Enter so it is stored in the default OS location.

Add ssh key to ssh agent:

    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_rsa

Copy ssh key to add it to your github/gitlab account:

    cat ~/.ssh/id_rsa.pub 

Go to Github/Gitlab settings and add public ssh key

Test connection:

    ssh -T git@github.com

Clone your repo:
    
    git clone git@github.com:username/repository.git 



