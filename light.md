1. git clone https://github.com/wenry55/mmpose.git. 
2. mmpose document에 나와있는대로 mmpose를 설치합니다.  
3. A100의 mmpose 아래에 있는 다음의 파일을 DAD의 mmpose 밑에 복사합니다.

   configs/animal/2d_kpt_sview_rgb_img/topdown_heatmap/animalpose/bk_mobilenet.py 

4. A100에 있는 아래의 파일의 DAD의 mmpose 밑에 같은 경로에 복사합니다.
   
   work_dirs/bk_mobilenet/epoch_180.pth
   
5. 아래를 실행하면 됩니다. 아래 프로그램은 input으로 video를 받아서 pose estimation 까지 하고, 처리 프레임 숫자를 console에 찍어줍니다.
   dad에서는 적당한 mp4화일 하나를 --video-path에 파라메터로 주기 바랍니다.
   
python3 demo/top_down_pose_tracking_demo_with_mmdet.py \
    ../mmdetection/configs/yolo/yolov3_mobilenetv2_mstrain-416_300e_coco.py \
    https://download.openmmlab.com/mmdetection/v2.0/yolo/yolov3_mobilenetv2_mstrain-416_300e_coco/yolov3_mobilenetv2_mstrain-416_300e_coco_20210718_010823-f68a07b3.pth \
    configs/animal/2d_kpt_sview_rgb_img/topdown_heatmap/animalpose/bk_mobilenet.py \
    work_dirs/bk_mobilenet/epoch_180.pth \
    --video-path=[mp4 파일(비디오)의 경로] --out-video-root mobilenet_640_Short --det-cat-id 20
    
    
