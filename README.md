# OpenCV4Cuda102Ubuntu18
Installation OpenCV 4.2.0 with CUDA 10.2 CUDNN 7.6.5 on UBUNTU 18.04 (VIDEO CODEC SDK) - NVIDIA GEFORCE GTX 1060 (6GB)
 1. Step: Install CUDA 10.2
 https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=deblocal
 Operating System : Linux
 Architecture : x86_64
 Distribution : Ubuntu
 Version : 18.04
 Installer Type : deb(local)
 # Installation Instructions:

$ wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin

$ sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600

$ wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb

$ sudo dpkg -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb

$ sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub

$ sudo apt-get update

$ sudo apt-get -y install cuda

$ sudo nano ~/.bashrc
# Adding end to file

$ export PATH=/usr/local/cuda-10.2/bin:$PATH

$ export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64:$LD_LIBRARY_PATH

$ source ~/.bashrc

$ sudo reboot

$ nvcc -V

# If you get "nvcc not found" error, the installation is not done correctly. Please don't do "sudo apt install nvidia-cuda-toolkit". Because in this case it is not seen by opencv.
# Installation CUDNN
# https://developer.nvidia.com/rdp/cudnn-archive --> Download cuDNN 7.6.5 Library for Linux (a .tgz file)
# Cudnn files from export .tgz file

$ cd ~/cudnn-10.2-linux-x64-v7.6.5

$ sudo cp ./cuda/lib64/* /usr/local/cuda/lib64/

$ sudo cp ./cuda/include/* /usr/local/cuda/include/

$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*

# Installation Video Codec SDK 
# https://developer.nvidia.com/nvidia-video-codec-sdk
# Download NVDECODE 

$ sudo cp ~/Video_Codec_SDK_11.0.10/Lib/linux/stubs/x86_64/libnvi* /usr/local/cuda/lib64/

$ sudo cp ~/Video_Codec_SDK_11.0.10/Interface/* /usr/local/cuda/include/

# Now, starting installation OPENCV 4.2.0

$ sudo apt update

$ sudo apt upgrade

$ sudo apt install build-essential cmake pkg-config unzip yasm git checkinstall

$ sudo apt install libjpeg-dev libpng-dev libtiff-dev

$ sudo apt install libavcodec-dev libavformat-dev libswscale-dev libavresample-dev

$ sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev

$ sudo apt install libxvidcore-dev x264 libx264-dev libfaac-dev libmp3lame-dev libtheora-dev 

$ sudo apt install libfaac-dev libmp3lame-dev libvorbis-dev

$ sudo apt install libopencore-amrnb-dev libopencore-amrwb-dev

$ sudo apt-get install libdc1394-22 libdc1394-22-dev libxine2-dev libv4l-dev v4l-utils

$ cd /usr/include/linux

$ sudo ln -s -f ../libv4l1-videodev.h videodev.h

$ cd ~


$ sudo apt-get install libgtk-3-dev

$ sudo apt-get install libtbb-dev

$ sudo apt-get install libatlas-base-dev gfortran

$ sudo apt-get install libprotobuf-dev protobuf-compiler

$ sudo apt-get install libgoogle-glog-dev libgflags-dev

$ sudo apt-get install libgphoto2-dev libeigen3-dev libhdf5-dev doxygen

$ cd ~

$ git clone https://github.com/opencv/opencv.git

$ git clone https://github.com/opencv/opencv_contrib.git

$ cd opencv/

$ git checkout -b v4.2.0

$ cd ..

$ cd opencv_contrib/

$ git checkout -b v4.2.0

$ cd ..

$ cd opencv/

# If you want to install with Qt you must add WITH_QT=ON
$ mkdir build && cd build

$ cmake -D CMAKE_BUILD_TYPE=RELEASE \    
-D CMAKE_INSTALL_PREFIX=/usr/local \         
-D INSTALL_C_EXAMPLES=ON \         
-D INSTALL_PYTHON_EXAMPLES=OFF \        
-D WITH_TBB=ON \     
-D WITH_CUDA=ON \      
-D CUDA_GENERATION="Pascal" \        
-D CUDA_ARCH_BIN=6.1 \     
-D BUILD_opencv_cudacodec=ON \      
-D ENABLE_FAST_MATH=ON \         
-D NVCUVID_FAST_MATH=ON \         
-D CUDA_FAST_MATH=ON \          
-D WITH_CUBLAS=ON \          
-D BUILD_opencv_java=OFF \          
-D BUILD_ZLIB=ON \          
-D BUILD_TIFF=ON \         
-D WITH_GTK=ON \          
-D WITH_NVCUVID=ON \          
-D WITH_FFMPEG=ON \        
-D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-10.2 \        
-D WITH_1394=ON \          
-D CUDNN_INCLUDE_DIR=/usr/local/cuda/include \   
-D CUDNN_LIBRARY=/usr/local/cuda/lib64/libcudnn.so.7.6.5 \    
-D OPENCV_GENERATE_PKGCONFIG=ON \        
-D OPENCV_PC_FILE_NAME=opencv.pc \        
-D OPENCV_PC_FILE_NAME=opencv4.pc \        
-D OPENCV_ENABLE_NONFREE=ON \         
-D WITH_GSTREAMER=ON \         
-D WITH_V4L=ON \          
-D WITH_QT=ON \         
-D WITH_CUDNN=ON \         
-D OPENCV_DNN_CUDA=ON \          
-D WITH_OPENGL=ON \          
-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \         
-D BUILD_EXAMPLES=ON ..

$ make -j4
$ sudo make install 
$ sudo ldconfig
$ pkg-config --libs opencv4

#Output: -L/usr/local/lib -lopencv_gapi ... -lopencv_cudafeatures2d ...

# Optional Install 
$ sudo apt-get install qtcreator
$ sudo apt-get install qt5-default
$ sudo apt-get update

# READY SYSTEM 
