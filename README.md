# jetson_config

## 설정목표

DAD(xavier nx) 장비의 edge 에서의 활용가능성을 판단하기 위하여,

1. mmdetection / mmpose 를 설치하고,
2. pose estimation시의 성능측정

## 측정결과

0.507 (frame/sec)

이미지 사이즈 640x480을 기준으로

detection 에 소요되는 시간 : 1.32  
pose estimation 에 소요되는 시간 : 0.65

한 프레임 처리에 1.97 초

## 설정에 사용된 커맨드 

(좌측의 순서는 원래 사용되었던 커맨드의 순서이나, 다음 설치시 편의를 위하여 조정하였음.)

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

## 설치시 주의사항

mmpose / mmdetection을 설치하기 위해서 필요한 lib/module을 설치하기 위해서는 꽤 많은 시행착오가 존재하였음.  
1. pytorch 는 최초에 1.3을 사용하려고 하였으나 1.7로 최종확정함. 이는 현재 jetpack 설치시 cuda 10.2 가 설치됨에 기인함.  
2. numpy는 1.19.4로 설치해야함. 1.19.5설치시 많은 오류가 발생함.  
3. python3 를 사용해야함.  
4. mmcv 설치시 3시간 정도 소요됨.  
