# jetson_config

## 설정목표

mmdetection / mmpose 를 설치

## 설정환경

1. Python 3.6.9
	2. /usr/bin/python3 -m pip install --upgrade pip
	3. Download pytorch wheel
	
wget https://nvidia.box.com/shared/static/phqe92v26cbhqjohwtvxorrwnmrnfx1o.whl
	
	(https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-9-0-now-available/72048)
	
	4. Rename above to torch-1.3.0-cp36-cp36m-linux_aarch64.whl
	5. Pip install torch-1.3.0-cp36-cp36m-linux_aarch64.whl

Python 3.6.9 (default, Oct  8 2020, 12:12:24) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/lib/python3.6/dist-packages/torch/__init__.py", line 81, in <module>
    from torch._C import *
ImportError: libnvToolsExt.so.1: cannot open shared object file: No such file or directory
>>> 

https://forums.developer.nvidia.com/t/how-can-i-install-the-pytorch/108038

Hi lee2h, which version of JetPack are you using? Those PyTorch wheels should be installed on JetPack 4.2 or newer.
ImportError: libnvToolsExt.so.1: cannot open shared object file: No such file or directory
libnvToolsExt.so should be installed by CUDA toolkit under /usr/local/cuda/lib64 - so either the SDK Manager did not install CUDA toolkit to your TX2, or you should add these lines to the end of your ~/.bashrc file:
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}


SDK Manager로 설치하기 힘듦

SDK사이즈가 큰데, 공간이 그정도 없음.
발열이 제법 큼 - 하루정도 지난뒤 만지다가 데일뻔.

Recovery mode 로 진입해야 sdk manager가 인식할 수 있음.


install jetpack without sdk manager
https://docs.nvidia.com/jetson/jetpack/install-jetpack/index.html


새로 받은것에는 아래의 버전이 설치되어 있음.
nvidia-jetpack is already the newest version (4.5.1-b17).

다시 파이토치 테스트

이번엔 pip가 설치되어 있지 않음.

Wget https://bootstrap.pypa.io/get-pip.py

=> python3 -m get-pip.py 



bnd@bnd-desktop:~/skcc$ python3
Python 3.6.9 (default, Jan 26 2021, 15:33:00) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/bnd/.local/lib/python3.6/site-packages/torch/__init__.py", line 81, in <module>
    from torch._C import *
ImportError: libcudart.so.10.0: cannot open shared object file: No such file or directory
>>> 



export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

10.2가 cuda로 링크되어 있어서 .bashrc를 다음과 같이 수정.

위와 같이 해도  오류가 발생

Libcudart.so.10.0 

그러므로 버전을 10.2를 지원하는것을 설정해야함.

Pytorch 1.7을 설치하기로 함.
https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-9-0-now-available/72048
https://nvidia.box.com/shared/static/cs3xn3td6sfgtene6jdvsxlr366m2dhq.whl


Python 3.6.9 (default, Jan 26 2021, 15:33:00) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/bnd/.local/lib/python3.6/site-packages/torch/__init__.py", line 189, in <module>
    _load_global_deps()
  File "/home/bnd/.local/lib/python3.6/site-packages/torch/__init__.py", line 142, in _load_global_deps
    ctypes.CDLL(lib_path, mode=ctypes.RTLD_GLOBAL)
  File "/usr/lib/python3.6/ctypes/__init__.py", line 348, in __init__
    self._handle = _dlopen(self._name, mode)
OSError: libmpi_cxx.so.20: cannot open shared object file: No such file or directory
>>> 


For others who may read this thread - you have to run this before installing the PyTorch wheel: sudo apt-get install libopenblas-base libopenmpi-dev See here for more instructions:   https://forums.developer.nvidia.com/t/pytorch-for-jetson-nano-version-1-6-0-now-available/


Python 3.6.9 (default, Jan 26 2021, 15:33:00) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.__version__
'1.7.0'
>>> 


Python 3.6.9 (default, Jan 26 2021, 15:33:00) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.__version__
'1.7.0'
>>> torch.cuda.is_available()
True
>>> torch.randn(4,4,4).cuda().mean()
tensor(0.0757, device='cuda:0')
>>> 


Python3 -m pip install torchvision
Python3 -m pip install terminaltables
Python3 -m pip install Cython

은 이상없이 진행되나 pycocotools 가 설치안됨.

우선 넘어감.

Mmcv 설치


pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu102/torch1.7.0/index.html

    In file included from /home/bnd/.local/lib/python3.6/site-packages/torch/include/torch/csrc/Device.h:3:0,
                     from /home/bnd/.local/lib/python3.6/site-packages/torch/include/torch/csrc/api/include/torch/python.h:8,
                     from /home/bnd/.local/lib/python3.6/site-packages/torch/include/torch/extension.h:6,
                     from /tmp/pip-install-veatzxro/mmcv-full_1e4fd95df30e4062ab598a2f8cf60d06/mmcv/ops/csrc/pytorch_cpp_helper.hpp:3,
                     from ./mmcv/ops/csrc/pytorch/box_iou_rotated.cpp:4:
    /home/bnd/.local/lib/python3.6/site-packages/torch/include/torch/csrc/python_headers.h:10:10: fatal error: Python.h: No such file or directory
     #include <Python.h>
              ^~~~~~~~~~
    compilation terminated.
    error: command 'aarch64-linux-gnu-gcc' failed with exit status 1


설치시 에러남.

sudo apt-get install python3-dev  # for python3.x installs

설치후 다시 mmcv 인스톨 시도

Building wheels for collected packages: mmcv-full
  Building wheel for mmcv-full (setup.py) ... done
  Created wheel for mmcv-full: filename=mmcv_full-1.3.9-cp36-cp36m-linux_aarch64.whl size=26021705 sha256=f18f1f0211324b107992610950e76f5faf78a0a0e0bc5b9d69279562682647d5
  Stored in directory: /home/bnd/.cache/pip/wheels/71/42/96/eb5fdcc1f96f76b2e2a8c446ce00074c4b3272197e4c0b8b91
Successfully built mmcv-full
Installing collected packages: mmcv-full
  WARNING: Value for scheme.headers does not match. Please report this to <https://github.com/pypa/pip/issues/10151>
  distutils: /home/bnd/.local/include/python3.6m/mmcv-full
  sysconfig: /home/bnd/.local/include/python3.6/mmcv-full
  WARNING: Additional context:
  user = True
  home = None
  root = None
  prefix = None
WARNING: Value for scheme.headers does not match. Please report this to <https://github.com/pypa/pip/issues/10151>
distutils: /home/bnd/.local/include/python3.6m/UNKNOWN
sysconfig: /home/bnd/.local/include/python3.6/UNKNOWN
Successfully installed mmcv-full-1.3.9


거의 두시간 걸림.

git clone https://github.com/open-mmlab/mmdetection.git
cd mmdetection

Python3 -m pip install mmdet를 바로 실행시

Matplotlib에서 걸림

sudo apt-get install python3-matplotlib

으로 처리.


테스트 결과 

640x427 파일 처리

1.35초당 1장

Xtcocotools 설치오류 해결방법

Numpy 1.19.5 를 쓰려면
OPENBLAS_CORETYPE=ARMV8 python3
또는 export OPENBLAS_CORETYPE=ARMV8
Or .bashrc 에 OPENBLAS_CORETYPE=ARMV8 추가

아니면 numpy를 1.19.4 로 설치

bnd@bnd-desktop:~/skcc/git/xtcocoapi$ python3 setup.py build_ext install
running build_ext
skipping 'xtcocotools/_mask.c' Cython extension (up-to-date)
running install
error: can't create or remove files in install directory

The following error occurred while trying to add or remove files in the
installation directory:

    [Errno 13] Permission denied: '/usr/local/lib/python3.6/dist-packages/test-easy-install-15702.write-test'

The installation directory you specified (via --install-dir, --prefix, or
the distutils default setting) was:

    /usr/local/lib/python3.6/dist-packages/

Perhaps your account does not have write access to this directory?  If the
installation directory is a system-owned directory, you may need to sign in
as the administrator or "root" account.  If you do not have administrative
access to this machine, you may wish to choose a different installation
directory, preferably one that is listed in your PYTHONPATH environment
variable.

For information on other options, you may wish to consult the
documentation at:

  https://setuptools.readthedocs.io/en/latest/easy_install.html

Please make the appropriate changes for your system and try again.

bnd@bnd-desktop:~/skcc/git/xtcocoapi$ sudo python3 setup.py build_ext install

Using /usr/lib/python3/dist-packages
Searching for Cython==0.29.24
Best match: Cython 0.29.24
Adding Cython 0.29.24 to easy-install.pth file
Installing cygdb script to /usr/local/bin
Installing cython script to /usr/local/bin
Installing cythonize script to /usr/local/bin

Using /home/bnd/.local/lib/python3.6/site-packages
Searching for setuptools==57.4.0
Best match: setuptools 57.4.0
Adding setuptools 57.4.0 to easy-install.pth file

Using /home/bnd/.local/lib/python3.6/site-packages
Finished processing dependencies for xtcocotools==1.9

 python3 -m pip install -r requirements.txt 
sudo python3 setup.py develop

Python3 -m pip install trimesh

ModuleNotFoundError: No module named 'poseval' git clone https://github.com/svenkreiss/poseval.git cd poseval pip install -e .


Poseval 은 다음의 라이브러리를 필요로 한다. 그렇지 않으면 쭉 내려간다.

OSError: Could not find library geos_c or load any of its variants ['libgeos_c.so.1', 'libgeos_c.so']

sudo apt-get install libgeos-dev


Install shapely


Defaulting to user installation because normal site-packages is not writeable
Collecting Shapely
  Using cached Shapely-1.7.1.tar.gz (383 kB)
Building wheels for collected packages: Shapely
  Building wheel for Shapely (setup.py) ... done
  Created wheel for Shapely: filename=Shapely-1.7.1-cp36-cp36m-linux_aarch64.whl size=777126 sha256=52dcdeb4da1f411132efce90e588df27c10ec8d08e2e9829b36b03e9b59c0db4
  Stored in directory: /home/bnd/.cache/pip/wheels/1c/f7/7f/c03026d77076c1d8f8dc2bdcd0cde51c338b0d791df6afd489
Successfully built Shapely
Installing collected packages: Shapely
  WARNING: Value for scheme.headers does not match. Please report this to <https://github.com/pypa/pip/issues/10151>
  distutils: /home/bnd/.local/include/python3.6m/Shapely
  sysconfig: /home/bnd/.local/include/python3.6/Shapely
  WARNING: Additional context:
  user = True
  home = None
  root = None
  prefix = None
WARNING: Value for scheme.headers does not match. Please report this to <https://github.com/pypa/pip/issues/10151>
distutils: /home/bnd/.local/include/python3.6m/UNKNOWN
sysconfig: /home/bnd/.local/include/python3.6/UNKNOWN
Successfully installed Shapely-1.7.1

![image](https://user-images.githubusercontent.com/3777314/127187719-518042c9-3d98-4441-8064-db40a6a2f9f7.png)


```
    1  sudo apt update
    2  sudo apt upgrade

    6  sudo apt install nvidia-jetpack
   14  sudo apt show nvidia-jetpack

   32  mkdir skcc
   33  cd skcc
   35  wget https://bootstrap.pypa.io/get-pip.py
   37  python3 get-pip.py 
   39  python3 -m pip

   48  sudo vi ~/.bashrc
   49  source ~/.bashrc

   52  wget https://nvidia.box.com/shared/static/cs3xn3td6sfgtene6jdvsxlr366m2dhq.whl
   55  mv cs3xn3td6sfgtene6jdvsxlr366m2dhq.whl torch-1.7.0-cp36-cp36m-linux_aarch64.whl
   56  python3 -m pip install torch-1.7.0-cp36-cp36m-linux_aarch64.whl 

   58  sudo apt-get install libopenblas-base libopenmpi-dev
  273  sudo apt-get install libgeos-dev
   85  sudo apt-get install python3-matplotlib
   70  sudo apt-get install python3-dev

   62  python3 -m pip install pycocotools
   63  python3 -m pip install torchvision
   64  python3 -m pip install terminaltables
   66  python3 -m pip install pycocotools

   69  python3 -m pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu102/torch1.7.0/index.html

   73  mkdir git
   74  cd git
   76  git clone https://github.com/open-mmlab/mmdetection.git

   81  cd ..
   82  python3 -m pip install mmdet
  182  python3 -m pip uninstall numpy
  183  python3 -m pip install numpy==1.19.4

   77  cd mmdetection/
   89  mkdir test
   90  cd test
  100  mkdir checkpoints
  101  cd checkpoints/
  102  wget https://download.openmmlab.com/mmdetection/v2.0/faster_rcnn/faster_rcnn_r50_fpn_1x_coco/faster_rcnn_r50_fpn_1x_coco_20200130-047c8118.pth
  103  cd ..

  105  vi verify.py 
  104  python3 verify.py 

  117  cd ..
  119  git clone https://github.com/open-mmlab/mmpose.git
  120  cd mmpose
  130  vi requirements.txt => remove 'optional'
  124  python3 -m pip install -r requirements.txt 
  126  python3 setup.py develop

# if xtcocotools not installed by pip install, use source in github.
  165  python3 -m pip install xtcocotools
  166  cd ..
  167  git clone https://github.com/jin-s13/xtcocoapi.git
  168  cd xtcocoapi/
  171  python3 -m pip install -r requirements.txt 
  172  python3 setup.py install
  177  python3 setup.py build_ext install
  179  OPENBLAS_CORETYPE=ARMV8 python3 setup.py build_ext install
  185  sudo python3 setup.py build_ext install


  194  python3 -m pip install -r requirements.txt 
  195  python3 setup.py develop
  196  sudo python3 setup.py develop
  198  python3 -m pip install trimesh
  201  sudo python3 -m pip install poseval

  234  python3 setup.py develop
  235  sudo python3 setup.py develop # => if sudo required in above command ...

  # test 
  253  python3 top_down_pose_tracking_demo_with_mmdet.py 
  254  python3 -m pip install mmpose
  255  python3 top_down_pose_tracking_demo_with_mmdet.py 

  # if 'poseval' is required on error ...
  256  cd ..
  257  git clone https://github.com/svenkreiss/poseval.git
  258  cd poseval/
  260  python3 -m pip install -e .


  # if Shapely need to be installed ...
  263  cd mmpose/
  271  python3 -m pip install Shapely

  # test mmpose
  279  python3 demo/top_down_img_demo.py  configs/body/2d_kpt_sview_rgb_img/topdown_heatmap/coco/hrnet_w48_coco_256x192.py  https://download.openmmlab.com/mmpose/top_down/hrnet/hrnet_w48_coco_256x192-b9e0b3ab_20200708.pth     --img-root tests/data/coco/ --json-file tests/data/coco/test_coco.json     --out-img-root vis_results
  197  python3 demo/top_down_img_demo.py  configs/animal/2d_kpt_sview_rgb_img/topdown_heatmap/macaque/res50_macaque_256x192.py  https://download.openmmlab.com/mmpose/animal/resnet/res50_macaque_256x192-98f1dd3a_20210407.pth     --img-root tests/data/macaque/ --json-file tests/data/macaque/test_macaque.json     --out-img-root vis_results


```
