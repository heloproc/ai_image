# [NVIDIA Deep Learning TensorRT Installation Guide](https://docs.nvidia.com/deeplearning/tensorrt/quick-start-guide/index.html#install)

## [Abstract](https://docs.nvidia.com/deeplearning/tensorrt/quick-start-guide/index.html#abstract)
This NVIDIA TensorRT 8.6.1 Quick Start Guide is a starting point for developers who want to try out TensorRT SDK; specifically, this document demonstrates how to quickly construct an application to run inference on a TensorRT engine.
For previously released TensorRT documentation, refer to the [TensorRT Archives](https://docs.nvidia.com/deeplearning/tensorrt/archives/index.html).

### 1. Introduction
  NVIDIA® TensorRT™ is an SDK for optimizing trained deep learning models to enable high-performance inference. TensorRT contains a deep learning inference optimizer for trained deep learning models, and a runtime for execution.
  After you have trained your deep learning model in a framework of your choice, TensorRT enables you to run it with higher throughput and lower latency.

  Here is a quick summary of each chapter:
* Installing TensorRT -
 We provide multiple, simple ways of installing TensorRT.


### 2. Installing TensorRT
There are a number of installation methods for TensorRT. This chapter covers the most common options using:
* a container
* a Debian file, or
* a standalone pip wheel file.
For other ways to install TensorRT, refer to the [NVIDIA TensorRT Installation Guide](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html).
>  For advanced users who are already familiar with TensorRT and want to get their application running quickly, who are using an NVIDIA CUDA® container with cuDNN included, or want to set up automation, follow the network repo installation instructions (refer to Using The [NVIDIA Machine Learning Network Repo For Debian Installation](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#maclearn-net-repo-install)).
 
 Ensure that you have the following dependencies installed:
 * [CUDA](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_local)
 * [cuDNN](https://docs.nvidia.com/deeplearning/cudnn/release-notes/rel_8.html#rel-890)
 * [Download](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#downloading) the TensorRT local repo file that matches the Ubuntu version and CPU architecture that you are using.
 * Install TensorRT from the Debian local repo package. Replace `ubuntuxx04`, `8.x.x`, and `cuda-x.x` with your specific OS version, TensorRT version, and CUDA version.
   ``` os="ubuntuxx04"
    tag="8.x.x-cuda-x.x"
    sudo dpkg -i nv-tensorrt-local-repo-${os}-${tag}_1.0-1_amd64.deb
    sudo cp /var/nv-tensorrt-local-repo-${os}-${tag}/*-keyring.gpg /usr/share/keyrings/
    sudo apt-get update
   ```
#### For full runtime
```
sudo apt-get install tensorrt
```
#### For the lean runtime only, instead of tensorrt
```
sudo apt-get install libnvinfer-lean8
sudo apt-get install libnvinfer-vc-plugin8
```

#### For lean runtime Python package
```
sudo apt-get install python3-libnvinfer-lean
```
#### For dispatch runtime Python package
```
sudo apt-get install python3-libnvinfer-dispatch
```
#### For all TensorRT Python packages
```
python3 -m pip install numpy
sudo apt-get install python3-libnvinfer-dev
```
The following additional packages will be installed:
* `
python3-libnvinfer`
* `
python3-libnvinfer-lean`
* `python3-libnvinfer-dispatch`

Verify the installation.
```
dpkg-query -W tensorrt
```
You should see something similar to the following:
`tensorrt	8.6.1.x-1+cuda12.0`



## Python Package Index Installation
This section contains instructions for installing TensorRT from the Python Package Index.
When installing TensorRT from the Python Package Index, you’re not required to install TensorRT from a `.tar`, `.deb`, or `.rpm` package. All required libraries are included in the Python package. However, the header files, which may be needed if you want to access TensorRT C++ APIs or to compile plugins written in C++, are not included. Additionally, if you already have the TensorRT C++ library installed, using the Python package index version will install a redundant copy of this library, which may not be desirable. Refer to Tar File Installation for information on how to manually install TensorRT wheels that do not bundle the C++ libraries. You can stop after this section if you only need Python support.


The tensorrt Python wheel files only support Python versions 3.6 to 3.11 at this time and will not work with other Python versions. Only the Linux operating system and x86_64 CPU architecture is currently supported. These Python wheel files are expected to work on CentOS 7 or newer and Ubuntu 18.04 or newer. While the tar file installation supports multiple CUDA versions, the Python Package Index installation does not and only supports CUDA 12.x in this release.

**Note:** If you do not have root access, you are running outside a Python virtual environment, or for any other reason you would prefer a user installation, then append `--user` to any of the `pip` commands provided.

## 1.Install the TensorRT Python wheel.
```python3 -m pip install --upgrade tensorrt```
The above pip command will pull in all the required CUDA libraries and cuDNN in Python wheel format from PyPI because they are dependencies of the TensorRT Python wheel. Also, it will upgrade tensorrt to the latest version if you had a previous version installed.

A TensorRT Python Package Index installation is split into multiple modules:
* TensorRT libraries (`tensorrt_libs`)
* Python bindings matching the Python version in use (`tensorrt_bindings`)
* Frontend source package, which pulls in the correct version of dependent TensorRT modules from pypi.nvidia.com (`tensorrt`)
Optionally, install the TensorRT lean or dispatch runtime wheels, which are similarly split into multiple Python modules. If you are only using TensorRT to run pre-built version compatible engines, you can install these wheels without installing the regular TensorRT wheel.

```
python3 -m pip install --upgrade tensorrt_lean
python3 -m pip install --upgrade tensorrt_dispatch
```

## 2.To verify that your installation is working, use the following Python commands to:
* Import the tensorrt Python module.
* Confirm that the correct version of TensorRT has been installed.
* Create a Builder object to verify that your CUDA installation is working.
 
```
 import tensorrt
 print(tensorrt.__version__)
 assert tensorrt.Builder(tensorrt.Logger())
```

If the final Python command fails with an error message similar to the error message below, then you may not have the NVIDIA driver installed or the NVIDIA driver may not be working properly.
