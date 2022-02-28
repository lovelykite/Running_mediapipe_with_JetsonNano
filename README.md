Install Mediapipe with JetsonNano
==================================
- Download JetPack 4.6 and flashed SD card (at least 32GB)
  - NVIDIA JetPack 4.6 : https://developer.nvidia.com/embedded/jetpack#install
  - Rufus : https://rufus.ie/en/

- Increase swap for more swap ram
~~~
$ git clone https://github.com/JetsonHacksNano/installSwapfile.git  
$ cd installSwapfile
~~~

- pre-install include Python and pip
~~~
$ sudo apt update
$ sudo apt-get update
$ sudo apt-get install python3-pip
$ sudo pip3 install -U pip testresources setuptools==49.6.0
~~~
