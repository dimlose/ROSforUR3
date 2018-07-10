# ROSforUR3
  Attach Files (for xtion)
# Freenect install (for kinect 1)
  http://wiki.ros.org/action/fullsearch/freenect_stack?action=fullsearch&context=180&value=linkto%3A%22freenect_stack%22


  https://github.com/ros-drivers/freenect_stack

  https://github.com/OpenKinect/libfreenect

  https://openkinect.org/wiki/Getting_Started



  Bridge
  https://github.com/OpenKinect/libfreenect/tree/master/OpenNI2-FreenectDriver







	A. Freenect for ros Driver install
	
	1. sudo apt-get install ros-XXXX-libfreenect
	2. https://github.com/ros-drivers/freenect_stack.git
	3. catkin_ws 에서 git clone, 빌드.

	B. libfreenect install
	
	1. 원하는 폴더에 git clone https://github.com/OpenKinect/libfreenect.git
	2. cd libfreenect
	    mkdir build
	    cd build
	    cmake -L ..
	    make
	
	    (/libfreenect/build 폴더 계속 유지)
	    cmake .. -DCMAKE_BUILD_PYTHON=ON
	    make
	
	    cmake .. -DCMAKE_BUILD_TYPE=debug
	    make
	
	   sudo make install
	    sudo ldconfig /usr/local/lib64/
	
	   sudo freenect-glview(<---SimpleViewer 같은 거 뜨면 성공)
	
	C. OpenNi2 for libfreenect bridge Driver
	
	(/libfreenect/build 폴더에서)
	cmake .. -DBUILD_OPENNI2_DRIVER=ON
	Make
	
	/libfreenect/build/lib/OpenNI2-FreenectDrivers/ 밑에 libFreenectDriver 파일들
	/OpenNI2/Bin/x64-Release/OpenNI2/Driver 밑에 복붙
	
	OpenNI2 SimpleViewer 도 돌아가면?
	
	D. Important!!!
	sudo apt-get install kinect-audio-setup
	reboot
	
	libfreenect/src/usb_libusb10.c <----------소스수정
	
	https://github.com/OpenKinect/libfreenect/issues/376
	like that
	
	
	In my model 1473 Kinect under ubuntu 14.04, in addition that using files from https://github.com/openframeworks/openFrameworks/tree/master/addons/ofxKinect/src/extra, src/usb_libusb10.c should be modified as followings, or else libusb_open function return -1 after calling libusb_reset_device(audioHandle) and libusb_close(audioHandle).
	libusb_device_handle * audioHandle = NULL;
res = libusb_open(audioDevice, &audioHandle);
if (res != 0)
{
     // we need to do this as it is possible that the device was not closed properly in a previous session
    // if we don't do this and the device wasn't closed properly - it can cause infinite hangs on LED and TILT functions
    libusb_reset_device(audioHandle);
    libusb_close(audioHandle);
    res = libusb_open(audioDevice, &audioHandle);                                                      
}
if (res != 0)
{                       
    FN_ERROR("Failed to set the LED of K4W or 1473 device: %d\n", res);
}
else
{
    #if 0   
    // we need to do this as it is possible that the device was not closed properly in a previous session
    // if we don't do this and the device wasn't closed properly - it can cause infinite hangs on LED and TILT functions
    libusb_reset_device(audioHandle);
    libusb_close(audioHandle);
    res = libusb_open(audioDevice, &audioHandle);
    #endif
    if (res == 0)
    {
	
	/libfreenect/build
	make
	
	끝.
	
