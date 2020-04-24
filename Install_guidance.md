## Installation

### Requirements:
- PyTorch 1.0 from a nightly release. It **will not** work with 1.0 nor 1.0.1. Installation instructions can be found in https://pytorch.org/get-started/locally/
- torchvision from master
- cocoapi
- yacs
- matplotlib
- GCC >= 4.9
- OpenCV
- CUDA >= 9.0

### Env: Ubuntu16.04

### step 1: CUDA and Dataset
```
# install Nvidia driver, version 384.130
# install CUDA >=9.0

# check if CUDA or cudnn installed
sudo apt list --installed | grep -E 'cuda|cudnn'

# Download coco dataset

```

### step 2: Python env
```bash
# first, make sure that your conda is setup properly with the right environment
# for that, check that `which conda`, `which pip` and `which python` points to the
# right path. From a clean conda env, this is what you need to do

conda create --name maskrcnn_benchmark -y
conda activate maskrcnn_benchmark

# this installs the right pip and dependencies for the fresh python
# this might install python3.8 that seems incompitable to version of PyTorch that we want to install
conda install ipython pip

# thus we need to revert python to 3.7
conda install python=3.7 anaconda=custom

# maskrcnn_benchmark and coco api dependencies
pip install ninja yacs cython matplotlib tqdm opencv-python
# some dependencies might have a wrong version
pip install torchvision==0.2.2
pip install pillow==6.2.1

# follow PyTorch installation in https://pytorch.org/get-started/locally/
# we give the instructions for CUDA 9.0
# this will install nightly version of 1.0.0xxx of pytorch
conda install -c pytorch pytorch-nightly torchvision cudatoolkit=9.0

export INSTALL_DIR=$PWD

# install pycocotools
cd $INSTALL_DIR
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
python setup.py build_ext install

# install cityscapesScripts
cd $INSTALL_DIR
git clone https://github.com/mcordts/cityscapesScripts.git
cd cityscapesScripts/
python setup.py build_ext install

# install apex
cd $INSTALL_DIR
git clone https://github.com/NVIDIA/apex.git
cd apex
# this apex might be too new for us, we neet to checkout an older version and makesure building successful
git checkout f3a960f80244cf9e80558ab30f7f7e8cbf03c0a0
python setup.py install --cuda_ext --cpp_ext

# install PyTorch Detection
# when dependencies change, we might need to delete build dir
cd $INSTALL_DIR
git clone https://github.com/facebookresearch/maskrcnn-benchmark.git
cd maskrcnn-benchmark

# the following will install the lib with
# symbolic links, so that you can modify
# the files if you want and won't need to
# re-build it
python setup.py build develop


unset INSTALL_DIR

# or if you are on macOS
# MACOSX_DEPLOYMENT_TARGET=10.9 CC=clang CXX=clang++ python setup.py build develop
```
