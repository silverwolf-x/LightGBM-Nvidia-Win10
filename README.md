# LightGBM GPU

20240429编译nvidia gpu成功存档

这里提供了GPU编译后的wheel，直接安装后在`test.ipynb`中测试，运行成功输出示例：

```
[LightGBM] [Info] This is the GPU trainer!!
[LightGBM] [Info] Total Bins 36
[LightGBM] [Info] Number of data points in the train set: 50, number of used features: 2
[LightGBM] [Info] Using GPU Device: NVIDIA GeForce RTX 4050 Laptop GPU, Vendor: NVIDIA Corporation
[LightGBM] [Info] Compiling OpenCL Kernel with 64 bins...
[LightGBM] [Info] GPU programs have been built
```

## Info

---

LightGBM version: 4.3.0（最新）

Operating System: Windows10 22H2

CPU: AMD Ryzen 7 7840H

**GPU: Nvidia** RTX 4050 Laptop

CUDA Toolkit: 12.4（最新）

Python: 3.12.3（编译后也能在3.10.14上运行）

---

CMake version: 3.29.2（最新）

Boost version: boost_1_85_0（最新）

**Build by Visual Studio 2022**

---

## Reference

[Win10 环境下，LightGBM GPU 版本的安装](https://zhuanlan.zhihu.com/p/55259112)

[GPU-Tutorials](https://lightgbm.readthedocs.io/en/latest/GPU-Tutorial.html) 

## 编译过程

[Win10 环境下，LightGBM GPU 版本的安装](https://zhuanlan.zhihu.com/p/55259112)有些措施需要更新，具体差不多

### 1. 下载与安装

- Git：在windows下使用shell命令运行LightGBM官方的`build-python.sh`

- [CMake](https://link.zhihu.com/?target=https%3A//cmake.org/download/)：安装时选择添加路径

![img](https://pic1.zhimg.com/80/v2-b8bb94e1d279ab7d0bc5d99e748adbac_1440w.webp)

- [Visual Studio 2022]([Thank You for Downloading Visual Studio Community Edition (microsoft.com)](https://visualstudio.microsoft.com/zh-hans/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030&passive=false))：在`使用C++的桌面开发`中，只需要安装下面两个组件
    - 最新的MSVC v143：编译工具
    - 最新的Windows SDK：这里包含编译要用的gcc

    拓展阅读：[Windows SDK 简介 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/622262355)

    > 微软官方推出的开发环境叫 Visual Studio，里面的 C/C++ 开发工具叫 Visual C++，编译器叫 MSVC，主要是 cl.exe 和 link.exe。大家一般就直接叫 VS 或 VC 了。
    >
    > 要编译出 C/C++程序，除了编译器，还需要一些头文件和链接库，这部分就是 SDK[[1\]](https://zhuanlan.zhihu.com/p/622262355#ref_1)[[2\]](https://zhuanlan.zhihu.com/p/622262355#ref_2)，同一个编译器可以配合不同的 SDK 用。

- [CUDA Toolkit 12.4 Update 1 Downloads | NVIDIA Developer](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exe_local)

​	只需要安装Toolkit部分，驱动可以不更新，Nsight可以忽略

- [Boost Binaries](https://sourceforge.net/projects/boost/files/boost-binaries/)最新版本的文件夹中，选择

    Visual Studio 2022：`msvc-14.3-64.exe`

​	安装到默认路径`C:\local\boost_1_85_0\`后可以不管环境变量的设置

### 2. 编译与报错处理

这里不同于[Win10 环境下，LightGBM GPU 版本的安装 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/55259112)，我们选择直接借助官方的`LightGBM/build-python.sh`进行编译

文档：[python-package#install-from-github](https://github.com/microsoft/LightGBM/tree/master/python-package#install-from-github)

首先clone最新源代码

```
git clone --recursive https://github.com/Microsoft/LightGBM
```

转到LightGBM文件夹下，打开git终端，运行下列命令。其中boost-include-dir换成你自己的，并且要用“**/**”输入地址

```
sh ./build-python.sh bdist_wheel --gpu --boost-include-dir=C:/local/boost_1_85_0/
```

报错：

>找不到 Windows SDK 版本 xxxx。请安装所 
>需版本的 Windows SDK，或者在项目属性页中或通过右键单击解决方案并选择“重定解决方案目标”来更改 SDK 版本。

[Visual Studio2019报错 错误 MSB8036 找不到 Windows SDK 版本 10.0.19041.0的解决方法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/496051774)

<details><summary>一些成功的运行日志</summary>

```text
--- building wheel ---  
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:
  - scikit-build-core>=0.4.4
* Getting build dependencies for wheel...
* Building wheel...
  2024-04-29 20:07:00,803 - scikit_build_core - INFO - RUN: C:\Program Files\CMake\bin\cmake.EXE -E capabilities
  2024-04-29 20:07:00,853 - scikit_build_core - INFO - CMake version: 3.29.2
  *** scikit-build-core 0.9.2 using CMake 3.29.2 (wheel)
  2024-04-29 20:07:00,887 - scikit_build_core - INFO - Build directory: C:\Users\Administrator\AppData\Local\Temp\tmp4x41bvzh\build
  *** Configuring CMake...
  2024-04-29 20:07:00,957 - scikit_build_core - WARNING - Can't find a Python library, got libdir=None, ldlibrary=None, multiarch=None, masd=None
  2024-04-29 20:07:00,959 - scikit_build_core - INFO - RUN: C:\Program Files\CMake\bin\cmake.EXE -S. -BC:\Users\ADMINI~1\AppData\Local\Temp\tmp4x41bvzh\build -CC:\Users\ADMINI~1\AppData\Local\Temp\tmp4x41bvzh\build\CMakeInit.txt -DUSE_GPU=ON -DBoost_INCLUDE_DIR='C:/local/boost_1_85_0/' -D__BUILD_FOR_PYTHON:BOOL=ON
  loading initial cache file C:\Users\ADMINI~1\AppData\Local\Temp\tmp4x41bvzh\build\CMakeInit.txt
  -- Building for: Visual Studio 17 2022
  -- Selecting Windows SDK version 10.0.19041.0 to target Windows 10.0.19045.
  -- The C compiler identification is MSVC 19.39.33523.0
  -- The CXX compiler identification is MSVC 19.39.33523.0
  -- Detecting C compiler ABI info
  -- Detecting C compiler ABI info - done
  -- Check for working C compiler: C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.39.33519/bin/Hostx64/x64/cl.exe - skipped
  -- Detecting C compile features
  -- Detecting C compile features - done
  -- Detecting CXX compiler ABI info
  -- Detecting CXX compiler ABI info - done
  -- Check for working CXX compiler: C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.39.33519/bin/Hostx64/x64/cl.exe - skipped
  -- Detecting CXX compile features
  -- Detecting CXX compile features - done
  -- Found OpenMP_C: -openmp (found version "2.0")
  -- Found OpenMP_CXX: -openmp (found version "2.0")
  -- Found OpenMP: TRUE (found version "2.0")
  -- Looking for CL_VERSION_3_0
  -- Looking for CL_VERSION_3_0 - found
  -- Found OpenCL: C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.4/lib/x64/OpenCL.lib (found version "3.0")
  -- OpenCL include directory: C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.4/include
  CMake Warning at C:/Program Files/CMake/share/cmake-3.29/Modules/FindBoost.cmake:1398 (message):
  New Boost version may have incorrect or missing dependencies and imported
  targets
  Call Stack (most recent call first):
  C:/Program Files/CMake/share/cmake-3.29/Modules/FindBoost.cmake:1523 (_Boost_COMPONENT_DEPENDENCIES)
  C:/Program Files/CMake/share/cmake-3.29/Modules/FindBoost.cmake:2135 (_Boost_MISSING_DEPENDENCIES)
  CMakeLists.txt:178 (find_package)
```
</details>
最后日志输出

```text
*** Making wheel...
2024-04-29 20:07:42,228 - scikit_build_core - INFO - Discovered Python package at lightgbm
*** Created lightgbm-4.3.0.99-py3-none-win_amd64.whl...
Successfully built \dist
cleaning up
```

生成的包在`\dist`中

### 3. 安装与测试

执行绝对路径安装,例如

```
pip install C:\LightGBM\dist\lightgbm-4.3.0.99-py3-none-win_amd64.whl
```

然后执行`test.ipynb`示例即可

## 关于GPU的更多参数选项

[GPU SDK Correspondence and Device Targeting Table — LightGBM 4.3.0 documentation](https://lightgbm.readthedocs.io/en/v4.3.0/GPU-Targets.html)

## 一些发现

实际上，直接`pip install lightgbm`下载官方包后，似乎可以直接自动编译？由于我的笔记本有双显卡

- AMD Radeon 780M Graphic（核心显卡）
- Nvidia RTX 4050 Laptop（独立显卡）

在非独显直连的模式下，第一次运行`test.ipynb`后显示：

```
4.3.0
[LightGBM] [Info] Number of positive: 29, number of negative: 21
[LightGBM] [Info] This is the GPU trainer!!
[LightGBM] [Info] Total Bins 36
[LightGBM] [Info] Number of data points in the train set: 50, number of used features: 2
[LightGBM] [Info] Using GPU Device: gfx1103, Vendor: Advanced Micro Devices, Inc.
[LightGBM] [Info] Compiling OpenCL Kernel with 64 bins...
[LightGBM] [Info] GPU programs have been built
[LightGBM] [Info] Size of histogram bin entry: 8
[LightGBM] [Info] 2 dense feature groups (0.00 MB) transferred to GPU in 0.000370 secs. 0 sparse feature groups
[LightGBM] [Info] [binary:BoostFromScore]: pavg=0.580000 -> initscore=0.322773
[LightGBM] [Info] Start training from score 0.322773
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
```

有一个很快的自动编译（我只安装了minimal版本的驱动），编译完成后，即使调整超参数`gpu_platform_id`也无法再切换为Nvidia

## 碎碎念

其实事情的背景是使用`autogluon`时，总是提示

```
"Warning: GPU mode might not be installed for LightGBM, GPU training raised an exception. Falling back to CPU training..."
"Refer to LightGBM GPU documentation: https://github.com/Microsoft/LightGBM/tree/master/python-package#build-gpu-version"
"One possible method is:"
"\tpip uninstall lightgbm -y"
"\tpip install lightgbm --install-option=--gpu"
```

以为是版本问题，于是折腾一大圈来编译安装，结果仍然报错。

最后翻查源码，原来是在训练lightgbm模型时，只要报错，就一律改为CPU运行。至于是什么报错，被掩盖起来不知道。

```python
try:
    self.model = train_lgb_model(early_stopping_callback_kwargs=
                                 early_stopping_callback_kwargs,
                                 **train_params)
except LightGBMError:
    if train_params["params"].get("device", "cpu") != "gpu":
        raise
    else:
        logger.warning(
            "Warning: GPU mode might not be installed for LightGBM, GPU training raised an exception. Falling back to CPU training..."
            "Refer to LightGBM GPU documentation: https://github.com/Microsoft/LightGBM/tree/master/python-package#build-gpu-version"
            "One possible method is:"
            "\tpip uninstall lightgbm -y"
            "\tpip install lightgbm --install-option=--gpu")
        train_params["params"]["device"] = "cpu"
        self.model = train_lgb_model(
            early_stopping_callback_kwargs=
            early_stopping_callback_kwargs,
            **train_params)
```

只有对`.py文件主动停止运行时，才能看到原来是分箱问题

```
lightgbm.basic.LightGBMError: bin size 6978 cannot run on GPU
```

这个bug上游已经新建文件夹了，还没修好[[Bug\] LightGBMError: bin size 257 cannot run on GPU · Issue #3339 · microsoft/LightGBM (github.com)](https://github.com/microsoft/LightGBM/issues/3339)只能作罢。

> 有一说一，有时GPU训练不一定比CPU快多少
