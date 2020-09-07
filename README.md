20.09.07 google colab에서 webcam 켜는 코드 추가

## Darknet을 colab에서 사용하기

<hr>

colab pro를 사용하여 GPU는 Tesla V100-SXM2-16GB  로 나오게 된다.

```python
!nvcc --version
print("===============================================================================")
!nvidia-smi
```

```python
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Sun_Jul_28_19:07:16_PDT_2019
Cuda compilation tools, release 10.1, V10.1.243
===============================================================================
Thu Sep  3 05:17:43 2020       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 450.66       Driver Version: 418.67       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla V100-SXM2...  Off  | 00000000:00:04.0 Off |                    0 |
| N/A   35C    P0    23W / 300W |      0MiB / 16130MiB |      0%      Default |
|                               |                      |                 ERR! |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

<hr>

## Colab에서 구동하는 코드를 내 깃허브에 올릴때

https://github.com/AlexeyAB/darknet 이 사람의 코드를 가져와서 나에게 맞게끔 수정후 깃허브에 올릴때는 .git 폴더를 삭제후 push 해야 한다. 안그러면 충돌남 !!

<hr>

## git bash창에서 퍼미션 변경하기

alexyab_new.ipynb 코드 실행시 `!./build.sh` 이 코드에서 문제가 있었는데 git bash 창에서 
`git ls-tree HEAD` 이 명령어 사용하면 아래와 같이 나온다.

```bash
user@YOURPCNAME ~/anyfolder (master)
$ git ls-tree HEAD
100644 blob dfd5446b47dec059d2d8a3e3831ec95b707e0441    build.sh

```

build.sh 파일을 `ls -al` 명령어로 확인하면 
`-rwxr-xr-x 1 user 197121   2044  9월  3 14:34 build.sh*` 이렇게 퍼미션이 755인걸 확인할 수 있지만 github에 push, colab 환경에서 clone 해서 `ls -al` 명령어로 보면
`-rw-r--r--  1 root root    2044 Sep  3 05:41 **build.sh*** ` 퍼미션이 이런식으로 644로 변한걸 볼 수 있다. 

### github에 퍼미션 755인 상태로 올리는법!

```bash
user@YOURPCNAME ~/anyfolder (master)
$ cd .git

user@YOURPCNAME ~/anyfolder/.git (GIT_DIR!)
$ vim config
```

github에 올리려는 폴더로 이동후 `ls -al`로 숨겨져 있는 .git 폴더로 이동한다.

config 창에서 filemode가 true인지 확인하고 아니면 false로 수정한다.

```bash
$ git update-index --chmod=+x build.sh
```

이 명령어로 build.sh의 퍼미션을 755로 바꿀 수 있다.

```bash
$ git ls-tree HEAD
```

다시 확인해보면 아래와 같이 변한걸 볼 수 있다. 

```bash
user@YOURPCNAME ~/anyfolder (master)
$ git ls-tree HEAD
100755 blob dfd5446b47dec059d2d8a3e3831ec95b707e0441    build.sh
```

<hr>

## iteration map

iteration map을 보고 싶다면 -dont_show -map flag  반드시 줘야 한다. 

```python
!./darknet detector train build/darknet/x64/data/obj.data build/darknet/x64/cfg/yolov4train.cfg yolov4.conv.137 -gpu 1 -dont_show -map flag 
```

/content/darknet 폴더 아래 chart.png 라는 파일이 생긴다. 