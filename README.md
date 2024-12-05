# Video Motion Detection Parallelization Project

  

## 1. Introduction

The goal of this project is to parallelize the motion detection of a video using two approaches: **Pthreads** and **Fastflow**. The motion detection process involves the following steps:


1.  **Grey Filter**: Converts the frame to grayscale.

2.  **Blur Filter**: Applies a blur effect to reduce noise.

3.  **Motion Detection**: Compares the processed frame with the background (initial frame) to detect motion. If the difference exceeds a set threshold, motion is detected.


At the end of the execution, the program reports the number of frames that differ from the background. The task is parallelized as a stream problem since the input is a series of video frames, and the goal is to minimize service time.


## 2. Sequential Execution Measurements
The first step is to implement the sequential version of the program in order to measure the time each function takes to be executed. The table below reports, in microseconds, the time each function takes to be executed.

![Sequential Time](https://github.com/Erriquez97/Motion-detect-parallel/blob/main/images/Sequential%20time.png)


The first observation is that the **Read frame** function is the only one that cannot be parallelized. Based on this, the service time cannot be below **2600.8 μs**.
  

## 3. Approach

  

The project uses a **Farm model** for parallelization, which consists of three stages: grey filter, blur filter, and motion detection. Each stage is computed sequentially within the same thread, which avoids overhead from passing frames between stages and optimizes memory locality.

  

### Service Time Analysis

  

The service time for the farm model is calculated as:

S(Farm) = max { T_read, T_grey + T_blur + T_motion }

  
  

Where:

-  `T_read`: Time to read the frame.

-  `T_grey`, `T_blur`, and `T_motion`: Times for applying the grey filter, blur filter, and detecting motion, respectively.

  

For optimal performance, 1 thread is used for reading and 10 threads for processing the frames in the farm, with the bottleneck at the reading function, resulting in a total service time of 2600.8μs using 11 threads.

  

## 4. Project Structure

  

### Folders

  

-  **Include**: Contains `Functions.hpp` and the `ff` folder.

-  `Functions.hpp`: Defines functions for the grey and blur filters, as well as motion detection.

-  `ff`: Contains headers for the Fastflow library.

-  **Resources**: Includes two video files (720p and 1080p) for motion detection.

-  **Statistics.cpp**: Source file to compute performance metrics.

-  **VideoMotionDetect.cpp**: Main file for motion detection and completion time reporting.

  

## 5. Compilation

  

To compile the project, follow these steps:

  

```bash
$  cd  VideoMotionDetect/
$  mkdir  build && cd  build
$  cmake  ..
$  make
```
  

## 6. Execution

  

There  are  two  executables:  **VideoMotionDetect**  and  **Statistics**.

  

### 6.1 VideoMotionDetect

  

This  program  takes  four  inputs:

  

1.  **Number  of  threads**:  Set  to  `0`  for sequential execution, or a positive integer for parallel execution.

2.  **Fastflow**:  Set  to  `0`  for C++ threads or `1`  for Fastflow.

3.  **Scheduling (ondemand)**: Set to `0`  to  disable  ondemand  scheduling  or  `1`  to  enable  it  for  Fastflow.

4.  **Path  to  video**:  Path  to  the  video  file  to  check  for  motion.

  

**Example**: To run the program with 16 threads using Fastflow and ondemand scheduling on the 720p video:

  

```bash
$ ./VideoMotionDetect 16  1  1 ../Resources/Video720p.mp4
```
  
  

### 6.2 Statistics

This program takes two inputs:
  

1.  **Show video**: Set to `0` to disable video output, or `1` to display the video with motion detection results in the terminal.

2.  **Path to video**: Path to the video file to check for motion.

 
**Example**: To run the program and show the 1080p video:

  

```bash
$  ./Statistics  1  ../Resources/Video1080p.mp4
```

## 7. Results

### 7.1 Service Time
![Service Time](https://github.com/Erriquez97/Motion-detect-parallel/blob/main/images/Service%20time.png)
### 7.2 Speedup
![Speedup](https://github.com/Erriquez97/Motion-detect-parallel/blob/main/images/Speedup.png)
### 7.3 Efficiency
![Efficiency](https://github.com/Erriquez97/Motion-detect-parallel/blob/main/images/Efficiency.png)
### 7.4 Scalability
![Scalability](https://github.com/Erriquez97/Motion-detect-parallel/blob/main/images/Scalability.png)
