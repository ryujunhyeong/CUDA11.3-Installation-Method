## Linux20.04 CUDA 11.3 & CuDNN 8.2 설치 방법


## 🛠 기본 프로그램 설치 

```bash
sudo apt-get install gcc
sudo apt-get install g++
```

## 🛠 nouveau 비활성화
```bash
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```

## 🙋‍♀️ 비활성화 확인 방법!!
```bash
 cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf
```

### 👉 비활성화 성공시 출력 내용
```bash
blacklist nouveau
options nouveau modeset=0
```

### 👉 시스템 업데이트 및 재부팅
```bash
sudo update-initramfs -u
sudo reboot
```

## 🛠 CUDA Toolkit Install
```bash
wget https://developer.download.nvidia.com/compute/cuda/11.3.0/local_installers/cuda_11.3.0_465.19.01_linux.run
ubuntu-drivers devices # 그래픽 드라이브 최적 버전 추천
sudo apt install nvidia-driver-470 # 그래픽카드 버전에 맞는 드라이버 설치
sudo sh cuda_11.3.0_465.19.01_linux.run # 위에서 다운받은 cuda_toolkit

```
### 👉 Toolkit 설치시 참고

- Driver은 해제하고 설치해야함 ( 위에서 드라이버 설치를 완료했기 때문 )
- CUDA Samples 아래는 설치하지 않아도 됨

### 👉 PATH 설정
```bash
#bash 셸에 대한 설정
vi ~/.bashrc 

# vim에서 추가
export PATH=/usr/local/cuda-11.3/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

# bash 셸에 대한 설정 즉시 적용
source ~/.bashrc
```

### 🙋‍♀️ 설치 확인 방법
```bash
nvcc -V
nvidia-smi
```

## CUDNN 8.2 설치 방법
### 🙋‍♀️ CuDNN 다운로드 링크
#### 로그인 후 다운 가능 (cuDNN Library for Linux (x86_64) 다운하면 됨)
- [다운로드 링크](https://developer.nvidia.com/rdp/cudnn-download)
  
![스크린샷, 2023-11-21 11-39-11](https://github.com/ryujunhyeong/cuda11.3-installation-method/assets/70519298/bab96845-cecd-44ff-90ba-d15683a1ea14)

### 🛠 적용방법
```bash
# 다운받은 CUDNN 파일이 있는곳에서 해야함!!
sudo cp -P cuda/include/cudnn*.h /usr/local/cuda/include
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```
### 🙋‍♀️ 설치 확인 방법
```bash
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```


### 👉 비활성화 성공시 출력 내용
```bash
#define CUDNN_MAJOR 8
#define CUDNN_MINOR 2
#define CUDNN_PATCHLEVEL 0
--
#define CUDNN_VERSION (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

#endif /* CUDNN_VERSION_H */
```
![스크린샷, 2023-11-21 11-38-35](https://github.com/ryujunhyeong/cuda11.3-installation-method/assets/70519298/84722469-7778-4586-b012-01f82ae16cc6)

### 🙋‍♀️ 최종 확인
```bash
# 최종 설치 확인
﻿nvcc -V
nvidia-smi

# CUDA 사용 가능 확인
python3
import torch # torch가 설치 되어 있어야 함
torch.cuda.is_available()

👉 True # True가 나오면 CUDA 사용 가능 상태!!
```
![스크린샷, 2023-11-21 11-37-49](https://github.com/ryujunhyeong/cuda11.3-installation-method/assets/70519298/2efe5f68-9446-4330-9052-844e3a36347a)

