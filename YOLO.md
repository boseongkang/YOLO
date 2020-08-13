## YOLO, Darknet 

텐서플로우 1.14버젼 설치, 케라스 2.2.4버젼 설치 

코랩 마운트 설정 https://github.com/pjreddie/darknet 여기서 다크넷 환경 깃 클론해서 가져온다.

##### Darknet 이란! : C언어로 작성된 물체 인식 오픈 소스 신경망이다. 그중 YOLOv3신경망을 사용했다.  즉, 다크넷이라는 큰 툴에서 yolov3를 사용하여 conv와 pooling을 통해 학습을 하고 탐지는 darknet을 사용하는거 같음. 

`%cd /content/darknet
!make` : 이 코드로 다크넷 환경 구축

##### colab에서 GPU 사용시 런타임 - 런타임 유형변경 - 하드웨어 가속기를 GPU로 설정. 위 코드에서 !make로 다크넷 환경이 구축 되었다면 content/darknet 폴더 안에 Makefile이라는 파일이 있다.  

```python
GPU=0    →  GPU=1
CUDNN=0  → CUDNN=1
OPENCV=0 → OPENCV=1
OPENMP=0 → OPENMP=0
DEBUG=0  → DEBUG=0
```

##### 이렇게 수정해주고 다시 `!make` 명령어를 통해 다크넷 환경을 구축해주면 GPU로 사용가능하다. GPU 사용전에는 17초 정도 였지만 GPU 사용후 0.05~0.09초면 실행된다.



`!ls -al darknet` : 구축된 다크넷 확인 `-rwxr-xr-x 1 root root 730568 Aug 13 01:29 darknet`

<hr>

## 학습된 모델 파일 다운로드

`%cd /content/darknet`
`!wget wget https://pjreddie.com/media/files/yolov3.weights`

저 사이트에서 yolov3.weights 다운로드 받는다. 

<hr>

## Darknet으로 물체 탐지 실행

`%cd /content/darknet`
`!./darknet detect cfg/yolov3.cfg yolov3.weights data/chair.jpg`

darknet을 통해 탐지 하는데 그때 cfg폴더 안에 yolov3.cfg를 사용하고 가중치는 yolov3.weights를 사용하며 사진은 data/chair.jpg를 사용한다. 



data/coco.names 파일들에 적힌 물체들을 YOLOv3가 인식할 수 있다. 

<hr>

colab에서 GPU 사용시 런타임 - 런타임 유형변경 - 하드웨어 가속기를 GPU로 설정.

