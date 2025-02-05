## Introduction
This page explains how to build exposure render with Visual Studio 2017

## Dependencies
Exposure render depends on: CUDA, Qt and VTK.

Versions used in this update:
* CUDA 11.8
* Qt 5.15.2
* VTK 9.1.0
* Microsoft Visual Studio 2022

### CUDA
You can skip this step if you already have the CUDA SDK and toolkit installed

The necessary installers can be downloaded from: http://developer.nvidia.com/cuda-toolkit-40
* Download and install the GPU Computing SDK
* Download and install the CUDA Toolkit
* Download and install the developer drivers

### Qt
Skip this step if you already have installed Qt

* Download the Qt installer from: https://www.qt.io/download-qt-installer
* Under Qt->5.15.2, Make sure you check the box for MSVC 2019 64-bit
* Add an environment variable called QTDIR and set it to \<install dir\>\5.15.2\msvc2019_64

### VTK
VTK must be compiled with Qt support
* Download the VTK 9.1.0 sources from the VTK website: http://www.vtk.org/VTK/resources/software.html
* Extract the zip file (for instance c:\VTK\9.1.0)
* Run CMAKE and set the source dir to the VTK root dir
* Make sure to use the 64-bit compiler
* Make sure to check the Qt check box in the VTK settings
* For the GUI to run you need to build VTK with GUI support
* See https://www.vtk.org/Wiki/VTK/Configure_and_Build for additional info/help
* You might also consider to build the examples
* Generate the Visual Studio 2022 solution files
* Open the generated solution and build it, RelWithDebugInfo is advised

### env variable
* CUDA_PATH
* CUDA_PATH_V11_8
* QT_DEBUG_PLUGINS 1
* QT_QPA_PLATFORM_PLUGIN_PATH C:\Qt\5.15.2\msvc2019_64\plugins
* PATH C:\VTK-9.1.0-install-debug\bin C:\Qt\5.15.2\msvc2019_64\bin

### Exposure Render
* Run CMAKE and set the source dir to the Exposure Render Source dir
* Make sure to select the 64-bit compiler.
* After building, copy the presets directory from the source dir to the build dir.

## Notes
* It might be required to update the maximum MVC version number in CUDA code.:
	* In \<CUDA_install_dir\>\NVIDIA GPU Computing Toolkit\CUDA\v11.8\include\crt\host_config.h
	* Change the right hand side of the equation: "#if _MSC_VER < 1600 || _MSC_VER > 1914" to a version number high enough to include your used Visual Studio version
* In order to run Exposure Render from Visual Studio, you might have to set ExposureRender as the startup project.
* Executable might not start due to missing DLL's:
	* Copy all the dll files from \<VTK_build_dir\>bin\Debug to \<ExposureRender_build_dir\>\debug
	* Copy Qt5Cored, Qt5Guid, Qt5Widgetsd, Qt5Xmld dll from \<Qt_install_dir\>\5.12.2\msvc2019_64\bin to \<ExposureRender_build_dir\>\debug
* Examples folder, Icons folder, ReadMe.txt and contents of presets folder from Exposure Source directory might also have to be copied to \<ExposureRender_build_dir\>\debug
* Currently GPU Capability is set to 5.0. If incompatible with your GPU Capability (Can be seen on startup screen of Exposure Render):
	* In CMakeLists.txt:
	* Change 'SET(CUDA_NVCC_FLAGS "-arch=sm_50;${CUDA_NVCC_FLAGS}")', to 'SET(CUDA_NVCC_FLAGS "-arch=sm_XX;${CUDA_NVCC_FLAGS}")', where XX is your Capability.
* Directory of Icons and Examples should be with exe, and xmls in Presets should be copied to exe directory.