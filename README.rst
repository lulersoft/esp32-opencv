================
OpenCV for Esp32
================


This is a clone of OpenCV (from commit 8808aaccffaec43d5d276af493ff408d81d4593c), modified to be cross-compiled on the ESP32. This Readme explains how to cross-compile on the ESP32 and also some details on the steps done. 



Hardware
========

The tests were done on the ESP32D0WDQ6 (revision 1):

- Xtensa dual core 32-bit LX6 uP, up to 600 MIPS
- 448 KB of ROM for booting and core functions
- 520 KB of SRAM for data and instructions cache
- 16 KB SRAM in RTC
- 8 MB of external SPI RAM
- 16 MB of external SPI Flash



Benchmark
=========

Below is a summary of the OpenCV features tested on the ESP32 and the time they took (adding the heap/stack used could also be useful).

All measures are in **milliseconds**.

+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| Function name and arguments                    | BUILD_TYPE=Debug                                      | BUILD_TYPE=Release                                    |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
|                                                |     160x120 |     320x240 |     640x480 |    1024x768 |     160x120 |     320x240 |     640x480 |    1024x768 |
+================================================+=============+=============+=============+=============+=============+=============+=============+=============+
|                                                                                                        |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| **Threshold**                                                                                          |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| binaryThreshold                                |         4.5 |          18 |          69 |         175 |         2.5 |          11 |          42 |         107 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| triangleThreshold                              |         8.0 |          32 |         124 |         315 |         3.9 |          17 |          66 |         168 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| OTSUThreshold                                  |          11 |          35 |         127 |         318 |         6.5 |          20 |          69 |         171 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| toZeroThreshold                                |         4.5 |          18 |          69 |         175 |         2.6 |          11 |          42 |         107 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
|                                                                                                        |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| **Blurring**                                                                                           |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| GaussianBlur 3x3 kernel                        |          16 |          54 |         206 |         521 |         5.6 |          20 |          76 |         192 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| medianBlur 3x3 kernel                          |         180 |         721 |        2883 |        7390 |          22 |          90 |         360 |         926 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| bilateralFilter diameter=5                     |         132 |         509 |        2014 |        5079 |          51 |         190 |         743 |        1854 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
|                                                                                                        |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| **Morphological tranforms**                                                                            |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| erode 5x5 kernel                               |          41 |         151 |         587 |        1494 |         6.2 |          22 |          84 |         214 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| dilate 5x5 kernel                              |          41 |         151 |         588 |        1495 |         6.2 |          22 |          84 |         214 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| open 5x5 kernel                                |          82 |         299 |        1164 |        2961 |          11 |          41 |         158 |         400 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
|                                                                                                        |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| **Resize image**                                                                                       |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| resize linear interpolation                    |          10 |          40 |         150 |         378 |         3.8 |          16 |          59 |         147 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| resize cubic interpolation                     |          21 |          75 |         287 |         728 |         6.5 |          27 |         108 |         275 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
|                                                                                                        |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| **Edge detection**                                                                                     |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| Sobel                                          |          34 |         116 |         438 |        1129 |          14 |          50 |         187 |         497 |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| Canny                                          |          80 |         256 |         886 |         ERR |          32 |         108 |         375 |         ERR |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
|                                                                                                        |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| **Hough tranformations**                                                                               |                                                       |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| HoughLines                                     |         392 |         897 |         ERR |         ERR |         314 |         686 |        2121 |         ERR |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+
| HoughLines probabilistic                       |         699 |        1652 |         ERR |         ERR |         603 |        1352 |        3765 |         ERR |
+------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+

The ``ERR`` fields means that the test hasn't pass (most of time due to OutOfMemory error).

The benchmark code can be found in `esp32/examples/esp_opencv_tests/`_.

.. _`esp32/examples/esp_opencv_tests/`: esp32/examples/esp_opencv_tests/

Installing esp-idf toolchain
============================

First thing to do is to install the toolchain for the esp32 (see https://docs.espressif.com/projects/esp-idf/en/latest/get-started/index.html)

.. code:: shell

  ### install some dependencies ###
  sudo apt-get install -y git wget libncurses-dev flex bison gperf python python-pip python-setuptools python-serial python-click python-cryptography python-future python-pyparsing python-pyelftools ninja-build ccache libffi-dev libssl-dev

  sudo apt-get install -y gawk gperf grep gettext python python-dev automake bison flex texinfo help2man libtool libtool-bin make git

  mkdir -p $INSTALLDIR && cd $INSTALLDIR
  git clone --recursive https://github.com/espressif/esp-idf.git

  cd esp-idf/
  export IDF_TOOLS_PATH=$INSTALLDIR
  ./install.sh
  export IDF_PATH=$INSTALLDIR/esp-idf
  . $INSTALLDIR/esp-idf/export.sh

This script can be found in `esp32/scripts/install_esp_toolchain.sh`_.

.. _`esp32/scripts/install_esp_toolchain.sh`: esp32/scripts/install_esp32_toolchain.sh


OpenCV cross-compilation:
=========================

This is the interesting part. OpenCV is statically cross-compiled. There are 3 ways to get it. 

Faster way: 
-----------

The first way is to simply get the pre-built OpenCV library in `esp32/lib/`_ folder, and copy it into your project (see Compiling-esp-idf-project-using-opencv)

.. _`esp32/lib/`: esp32/lib/


Fast way:
---------

The second way is by using the script in build_opencv_for_esp32.sh_. This script automatically compiles OpenCV from this repository sources, and install the needed files into the desired project. It can tweaked as needed to add and remove some parts. 

The script has 2 arguments. The first is the path to the  ``toolchain-esp32.cmake`` (default is ``$HOME/esp/esp-idf/tools/cmake/toolchain-esp32.cmake``), and the second is the path where the OpenCV library is installed (default is in esp32/lib).

.. _build_opencv_for_esp32.sh: esp32/scripts/build_opencv_for_esp32.sh

Detailed way:
-------------

The last way explains all the commands and modifications done to be able to compile and run OpenCV on the ESP32. The detailed procedure is in detailed_build_procedure.md_.

.. _detailed_build_procedure.md: esp32/doc/detailed_build_procedure.md


Get project RAM and Flash usages
===================================

At compilation time:
--------------------

- The command below can be used to see the different segments sizes of the application :

  .. code shell

    $ xtensa-esp32-elf-size -d -A build/<project-name>.elf

- The file ``build/<project-name>.map`` is also very useful. It indicates the memory mapping of the variables and can be used to find big variables in the application. 



At run time:
------------

.. code:: c++

  // Get the amount of stack (in Bytes) that remained unused when the task stack was at its greatest value
  ESP_LOGI(TAG, "task stack watermark: %d Bytes", uxTaskGetStackHighWaterMark(NULL));
  // Get the free heap in Bytes (may not be contiguous)
  ESP_LOGI(TAG, "heap left: %d Bytes", esp_get_free_heap_size());


Adding images codecs support
============================

Things done to read/writes images in JPEG, PNG, etc..

PNG
---

- Remove ``-DWITH_PNG=OFF`` and add ``-DBUILD_PNG=ON`` and ``-DBUILD_ZLIB=ON`` of the cmake command

  - The lib ``opencv_imgcodecs.a`` build pass

The library is compiled in the ``3rdparty/`` folder. Copy this folder into the esp32 example project folder.



JPEG
----

- Remove ``-DWITH_JPEG=OFF`` and add ``-DBUILD_JPEG=ON`` of the cmake command

  - Problem at compilation time. TODO



Adding parallel support
=======================

TODO



Removing OpenCV unnecessary parts 
=================================

Opencv is quite big, even when compiling only the core, imgproc and imgcodec modules. Because the ESP32 has limited resources, it is a good idea to remove some parts of opencv that are in most cases not used. 



TODO: List the modules functionalities and what is kept or not

Core module:
------------




Imgproc module:
---------------

Colorspaces
^^^^^^^^^^^

Opencv supports multiple colorspaces (RGB, BGR, RGB565, RGBA, CIELAB, CIEXYZ, Luv, YUV, HSV, HLS, YCrCb, Bayer, Gray). All these colorspaces are not mandatory for an embedded system, so some are removed.

- ``color_lab.cpp``: This file contains conversion for CIELAB and CIEXYZ (https://en.wikipedia.org/wiki/CIELAB_color_space). The conversion tables takes a lot of space in the .bss segment (~88kB) , which is already overflowing. Here are the steps done to disable this code:
  
  - Move ``color_lab.cpp`` to ``color_lab.cpp.bak`` 
  
  - In ``color.hpp`` disable :
  
    .. code:: c++

      // todo
    
  - In ``color.cpp`` disable:
  
    .. code:: c++

      // todo
  
- todo

