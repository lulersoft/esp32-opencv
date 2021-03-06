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
