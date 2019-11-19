This are Compress files of croscompiled OPENCV 4.1.0 for raspberry pi enviroment. 

instructions:

If you are using RPi Zero just change the name of the files accordingly.

Open a Terminal and download the desired version of OpenCV:

1 wget https://github.com/EdwinKestler/raspberry-pi-opencv/raw/master/pi3plus/opencv-4.1.0-armhf-no-gui.tar.bz2

Next, extract the archive:

1 tar xvf opencv-4.1.0-armhf.tar.bz2
Once the archive was extracted, move the resulting opencv-4.1.0 folder to /opt:

1 sudo mv opencv-4.1.0 /opt
Optionally, you can remove the archive:

1 rm opencv-4.1.0-armhf.tar.bz2
Next, we are going to install a couple of libraries required by OpenCV:

1 sudo apt install libtiff-dev zlib1g-dev
2 sudo apt install libjpeg-dev libpng-dev
3 sudo apt install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
4 sudo apt-get install libxvidcore-dev libx264-dev
5 sudo apt install python-numpy python3-numpy
The next two libraries are only required if you are using the GUI version of OpenCV, you can safely ignore these if you are using the headless version:

1 sudo apt install libgtk-3-dev libcanberra-gtk3-dev
Next, we’ll add OpenCV to the system library path, you’ll need to run these commands from your home folder:

1 cd ~
2 echo 'export LD_LIBRARY_PATH=/opt/opencv-4.1.0/lib:$LD_LIBRARY_PATH' >> .bashrc
3 . .bashrc
Restart your Terminal or log in and log out if you are connected to your RPi through SSH.

Next, let’s create some symbolic links that will allow Python to load the newly created libraries:

1 sudo ln -s /opt/opencv-4.1.0/lib/python2.7/dist-packages/cv2 /usr/lib/python2.7/dist-packages/cv2
2 sudo ln -s /opt/opencv-4.1.0/lib/python3.7/dist-packages/cv2 /usr/lib/python3/dist-packages/cv2
Install git if necessary:

1 sudo apt install git
We’ll clone a simple config file useful if you want to be able to use OpenCV from C++:

1 git clone https://gist.github.com/EdwinKestler/ed383474872958081985de733eaf352d opencv_cpp_compile_settings
2 cd opencv_cpp_compile_settings/
3 sudo cp opencv.pc /usr/lib/arm-linux-gnueabihf/pkgconfig
4 cd ~
5 rm -rf opencv_cpp_compile_settings/
At this point, you should be able to use the OpenCV library from C++ or Python.

On the repository for this article you can find a few C++ and Python test programs. You can download the code on your Pi with:

1 git clone https://github.com/EdwinKestler/raspberry-pi-opencv.git
2 cd raspberry-pi-opencv/tests
There are two headless tests that you can use even if you don’t have a display connected to your RPi: cli_cpp_test.cpp and cli_python_test.py. I’ve also included two graphical tests that require a display: gui_cpp_test.cpp and gui_python_test.py.

You can build and run the C++ tests like this:

1 g++ cli_cpp_test.cpp -o cli_cpp_test `pkg-config --cflags --libs opencv`
2 ./cli_cpp_test
or, if you have a display connected to your RPi:

1 g++ gui_cpp_test.cpp -o gui_cpp_test `pkg-config --cflags --libs opencv`
2 ./gui_cpp_test
Here is a screenshoot of the C++ GUI test running on my Pi:

Raspberri Pi C++ OpenCV 4 test

For the Python tests, use:

1 python3 cli_python_test.py
or

1 python3 gui_python_test.py