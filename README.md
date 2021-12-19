# Colorization-흑백사진 색채화
## Overview 
* 프로젝트 설명<br>
**1. RGB to RGB**<br>
  RGB의 gray image를 input으로 받아 RGB color image를 예측하는 방법<br><br>
**2. Lab to RGB**<br>
  Lab 색공간을 사용하여 밝기 채널 L을 input으로 받아 색상 채널 a,b로 mapping해주는 방법. 이후 시각화를 위해 Lab 색공간을 RGB 색공간으로 변환해줌<br><br><br>
 
* 사용한 모델<br>
**pix2pix:Image-to-Image Translation with Conditional Adversarial Network**<br>
<u>Generator</u><br>
![그림1](https://user-images.githubusercontent.com/65711055/146670653-dfebf137-c4d6-4e74-9126-14802eda463b.png)<br>
encoder와 decoder를 나란히 배치한 U-net 구조로 encoding을 거치면서 feature map이 점점 줄어들며 핵심적인 특징들만 추출되고, decoding 과정에서 다시 upsampling을 통해 원래의 영상으로 복원된다. 이때 compression을 진행하면서 날아간 정보를 보강해주기 위해서 skip connection으로 concat해준다.<br><br>
<u>Discriminator</u><br>
![그림2](https://user-images.githubusercontent.com/65711055/146670768-2114d764-7e3a-4494-8357-5ab61aa3bbb8.png)<br>
전체 영역의 이미지가 아니라 특정 크기의 patch 단위로 진짜 이미지인지 가짜 이미지인지를 판별하는 방식으로, 픽셀 간의 연관성이 거리에 반비례하다는 점을 이용하여 특정 크기의 patch에 대해 진짜 같은 이미지를 생성하여 generator의 성능을 높임<br><br>


## 과제 수행 방법
* Colab Pro 사용
* pytorch
* kaggle의 Landscape color and grayscale images 사용
* 영상 품질 평가 지표로는 PSNR, SSIM 사용

## hyperparameter
* image size는 256으로 resize
* gray, color image들의 픽셀은 -1~1 범위로 전처리
* batch_size = 32
* optimizer = Adam(lr = 2e-4, betas=(0.5,0.999))
* epoch = 150 (약 15시간 소요)
* 기존의 GAN loss에 L1 loss를 추가(lambda = 100)
* criterion = BCEwithLogitsLoss

## 실험
1. RGB to RGB: [여기로 이동](#english-must-be-small-capital)
2. RGB to RGB
3. RGB to RGB
4. Lab to RGB 
