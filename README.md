Install Mediapipe with JetsonNano
==================================
- Download JetPack 4.6 and flashed SD card (at least 32GB)
  - NVIDIA JetPack 4.6 : https://developer.nvidia.com/embedded/jetpack#install
  - Rufus : https://rufus.ie/en/

- To set Jetson Nano to 10W (MAXN) performance mode([reference](https://devtalk.nvidia.com/default/topic/1050377/jetson-nano/deep-learning-inference-benchmarking-instructions/)), execute the following from a terminal
~~~
$ sudo nvpmodel -m 0
$ sudo jetson_clocks
~~~

- Increase swap for more swap ram
~~~
$ git clone https://github.com/JetsonHacksNano/installSwapfile.git  
$ cd installSwapfile
$ ./installSwapfile.sh
~~~

- Pre-install include Python and pip
~~~
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install python3-pip
$ sudo pip3 install -U pip testresources setuptools==49.6.0
~~~

- Install python libraries for Tensorflow
~~~
sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran
~~~

- Install python packages
~~~
$ sudo pip3 install -U --no-deps numpy==1.19.4 future==0.18.2 mock==3.0.5 keras_preprocessing==1.1.2 keras_applications==1.0.8 gast==0.4.0 protobuf pybind11 cython pkgconfig
$ sudo env H5PY_SETUP_REQUIRES=0 pip3 install -U h5py==3.1.0
$ sudo apt-get install python3-opencv
~~~

- Install mediapipe v0.8.5 from the source code
~~~
$ git clone -b v0.8.5 https://github.com/google/mediapipe.git
$ cd mediapipe

$ sudo apt-get install -y libopencv-core-dev  libopencv-highgui-dev libopencv-calib3d-dev libopencv-features2d-dev libopencv-imgproc-dev libopencv-video-dev
$ sudo chmod 744 setup_opencv.sh
$ ./setup_opencv.sh
~~~

- Download mediapipe-bin and install
~~~
$ git clone https://github.com/PINTO0309/mediapipe-bin.git && cd mediapipe-bin
$ sudo chmod +x ./v0.8.5/download.sh && ./v0.8.5/download.sh
$ tar v0.8.5.zip
$ cd v0.8.5/numpy119x/py36
$ pip3 install mediapipe-0.8.5_cuda102-cp36-cp36m-linux_aarch64.whl
~~~

- Run sample hand detection
~~~
$ git clone https://github.com/Kazuhito00/mediapipe-python-sample && cd mediapipe-python-sample
$ python3 sample_hand.py
~~~

Solutions for errors during install and launch mediapipe
=========================================================
- Error Msg **"No module named 'mediapipe'"**
  - Visit https://github.com/jiuqiant/mediapipe_python_aarch64 and follow instructions.
 
 - Easiest ways to install bazel & protobuf
 ~~~
 $ git clone https://github.com/jkjung-avt/jetson_nano
 $ ./install_bazel-3.7.2.sh
 $ ./install_protobuf-3.9.2.sh
 ~~~
 
- Error Msg **"Target //mediapipe/modules/face_detection:face_detection_short_range_cpu failed to build"**
  - Update & install gcc-9
  ~~~
  $ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
  $ sudo apt update
  $ sudo apt install gcc-9 g++-9
  $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 100 --slave /usr/bin/g++ g++ /usr/bin/g++-9 --slave /usr/bin/gcov gcov /usr/bin/gcov-9
  ~~~
  - Clean the cache before you rerun the build command
  ~~~
  $ bazel clean --expunge
  ~~~
  - Run build code again
  ~~~
  sudo python3 setup.py bdist_wheel
  ~~~

- Error Msg **"An error occurred during the fetch of repository 'rules_cc':"**
  - Modify the WORKSPACE under the mediapipe root folder
  ~~~
  # line 37
  strip_prefix = "rules_cc-master",
  ↓
  strip_prefix = "rules_cc-main",
  
  urls = ["https://github.com/bazelbuild/rules_cc/archive/master.zip"],
  ↓
  urls = ["https://github.com/bazelbuild/rules_cc/archive/main.zip"],
  ~~~
  - Clean the cache before you rerun the build command
  ~~~
  $ bazel clean --expunge
  ~~~
  - Run build code again
  ~~~
  sudo python3 setup.py bdist_wheel
  ~~~

Reference
==========
- https://github.com/feitgemel/Jetson-Nano-Python
- https://github.com/PINTO0309/mediapipe-bin
- https://github.com/jiuqiant/mediapipe_python_aarch64
- https://github.com/jkjung-avt/jetson_nano
