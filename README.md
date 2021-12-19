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
 
