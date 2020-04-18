# Installing cuDNN On Windows

[reference](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#installwindows)

The following steps describe how to build a cuDNN dependent program. In the following sections the CUDA v9.0 is used as example:



1. Navigate to your <installpath> directory containing cuDNN.

2. Unzip the cuDNN package.

   ```
   cudnn-10.2-windows7-x64-v7.6.5.32.zip
   ```

   or

   ```
   cudnn-10.2-windows10-x64-v7.6.5.32.zip
   ```

3. Copy the following files into the CUDA Toolkit directory.

   

   1. Copy <installpath>\cuda\bin\cudnn64_7.6.5.32.dll to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\bin.
   2. Copy <installpath>\cuda\ include\cudnn.h to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\include.
   3. Copy <installpath>\cuda\lib\x64\cudnn.lib to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\lib\x64.

4. Set the following environment variables to point to where cuDNN is located. To access the value of the $(CUDA_PATH) environment variable, perform the following steps:

   

   1. Open a command prompt from the **Start** menu.

   2. Type Run and hit **Enter**.

   3. Issue the control sysdm.cpl command.

   4. Select the **Advanced** tab at the top of the window.

   5. Click **Environment Variables** at the bottom of the window.

   6. Ensure the following values are set:

      ```
      Variable Name: CUDA_PATH 
      Variable Value: C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2
      ```

5. Include cudnn.lib in your Visual Studio project.

   

   1. Open the Visual Studio project and right-click on the project name.
   2. Click **Linker > Input > Additional Dependencies**.
   3. Add cudnn.lib and click **OK**.

# Software requirements

[reference](https://www.tensorflow.org/install/gpu#software_requirements)

The following NVIDIA® software must be installed on your system:

- [NVIDIA® GPU drivers](https://www.nvidia.com/drivers) —CUDA 10.1 requires 418.x or higher.
- [CUDA® Toolkit](https://developer.nvidia.com/cuda-toolkit-archive) —TensorFlow supports CUDA 10.1 (TensorFlow >= 2.1.0)
- [CUPTI](http://docs.nvidia.com/cuda/cupti/) ships with the CUDA Toolkit.
- [cuDNN SDK](https://developer.nvidia.com/cudnn) (>= 7.6)
- *(Optional)* [TensorRT 6.0](https://docs.nvidia.com/deeplearning/sdk/tensorrt-install-guide/index.html) to improve latency and throughput for inference on some models.

# cudart_101.dll 找不到

[reference](https://github.com/tensorflow/tensorflow/issues/36111#issuecomment-581815799)

环境:

- windows 10 pro x64 1909
- CUDA v10.2
- tensorflow 2.1.0
- Python 3.7.6
- pip 20.0.2
- virtualenv 20.0.4
- GPU驱动程序版本: 441.22

网上下载cudart_101.dll, 放到`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\bin`