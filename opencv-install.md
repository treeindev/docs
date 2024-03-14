# Opencv C++ Installation in Ubuntu 
Installation tested on: ubuntu-22.04.4-live-server-arm64

## Installation Steps
Navigate to the home directory.
```
cd ~
```

Clone the `opencv` repository.
```
git clone https://github.com/opencv/opencv.git
```

Navigate and create the `release` folder.
```
cd ~/opencv && mkdir release && cd release
```

Include configurations for the compiler.
```
cmake -D CMAKE_BUILD_TYPE=Release -D GLIBCXX_USE_CXX11_ABI=0 -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

Compile the source code. This step might take several minutes.
```
make
```

Install `opencv` once the compilation finishes.
```
sudo make install
```

Create a basic `app.cpp` file and include c++ sintax importing opencv modules. Then run:
```
g++ app.cpp -I/usr/local/include/opencv4/ -lopencv_core -lopencv_imgcodecs -o app
```

## Fixing errors
If the following error shows on the console:<br>
<code>./main: error while loading shared libraries: libopencv_core.so.409: cannot open shared object file: No such file or directory</code>

```
You haven't put the shared library in a location where the loader can find it. look inside the /usr/local/opencv and /usr/local/opencv2 folders and see if either of them contains any shared libraries (files beginning in lib and usually ending in .so). when you find them, create a file called /etc/ld.so.conf.d/opencv.conf and write to it the paths to the folders where the libraries are stored, one per line.

For example, if the libraries were stored under /usr/local/opencv/libopencv_core.so.2.4 then I would write this to my opencv.conf file:

/usr/local/opencv/

Then run

sudo ldconfig -v

If you can't find the libraries, try running

sudo updatedb && locate libopencv_core.so.2.4

in a shell. You don't need to run updatedb if you've rebooted since compiling OpenCV.

References:

About shared libraries on Linux: http://www.eyrie.org/~eagle/notes/rpath.html

About adding the OpenCV shared libraries: http://opencv.willowgarage.com/wiki/InstallGuide_Linux
```

If the following error shows on the console:<br>
<code>main.cpp:2:10: fatal error: opencv2/core.hpp: No such file or directory
    2 | #include <opencv2/core.hpp></code>
<br>
Ensure that `-I/usr/local/include/opencv4/` is part of the parameters used while calling the `g++` compiler.

## References
https://docs.opencv.org/4.x/d7/d9f/tutorial_linux_install.html
https://stackoverflow.com/a/12335952
https://github.com/opencv/opencv/issues/13000

