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

<br><br>

# Machine Learning with Python

# Python Setup

# Git 


