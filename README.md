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
1. RGB to RGB: [1번 실험](#RGB_to_RGB.ipynb)
2. RGB to RGB: [2번 실험](#RGB_to_RGB.ipynb)
3. RGB to RGB: [3번 실험](#RGB_to_RGB.ipynb)
4. Lab to RGB: [4번 실험](#Lab_to_RGB.ipynb)

## 결과
**1. RGB to RGB - 1번실험**<br>
![loss1](https://user-images.githubusercontent.com/65711055/146713838-7c9b4a5d-cd7c-4b8a-9b9b-83928d568b0f.png)<br>
![실험1 그림1](https://user-images.githubusercontent.com/65711055/146713850-812d6937-b94e-44c2-8571-94108219a989.png)<br>
ssim:  0.9968320418069206, psnr = 63.6979<br>
![실험1 그림2](https://user-images.githubusercontent.com/65711055/146713858-332fa3e9-3e3a-4526-a75e-3143a1669b46.png)<br>
ssim:  0.9997259981233829, psnr = 69.8225<br>
![실험1 그림3](https://user-images.githubusercontent.com/65711055/146713866-d27ee26c-2d8d-4595-99fb-c9ec7bc4dfb7.png)<br>
ssim:  0.9998020751613257, psnr = 67.9700<br><br>
**2. RGB to RGB - 2번실험**<br>
![loss2](https://user-images.githubusercontent.com/65711055/146714294-46e37278-3541-44c0-b849-baeb08dc9c72.png)<br>
![실험2 그림1](https://user-images.githubusercontent.com/65711055/146714305-e0b82ff2-e851-4b85-b4c6-7c631647af82.png)<br>
ssim:  0.9999025335420327, psnr = 76.4366<br>
![실험2 그림2](https://user-images.githubusercontent.com/65711055/146714316-fd3bc8af-d13a-4194-9270-cabc47363579.png)<br>
ssim:  0.9999220159364776, psnr = 74.1495<br>
![실험2그림3](https://user-images.githubusercontent.com/65711055/146714336-1a646585-9f47-47f0-913f-86169e4bb83e.png)<br>
ssim:  0.9997991687728733 psnr =  69.5486<br>
**3. RGB to RGB - 3번실험**

**4. Lab to RGB - 4번실험**

## Conclusion
원래는 Colorizaiton을 위해 나온 모델인 ‘chromaGAN’까지 사용하는 것이 목표였는데, 처음 GAN을 사용하는 것이다 보니 미숙했던 점도 있었고, 색공간을 바꾸어가면서 시각화를 진행하려고 하다 보니 이 부분이 막혀서 시간이 많이 지체되어 목표를 달성하진 못했다. 하지만 pix2pix 모델을 사용하여 결과를 내기 위해 모델도 다양하게 바꾸어보고, activation function, 하이퍼파라미터, 전처리 방식도 다양하게 바꾸어가면서 사용해볼 수 있었기 때문에 더 깊이있게 이해할 수 있었던 것 같다. 특정한 부분에서 시간을 많이 잡아먹은 것은 아쉽지만, 이러한 경험을 통해서 앞으로 동일한 상황에 직면했을 때 문제를 더 효율적으로 해결할 수 있을 것 같다. 이 프로젝트는 GAN을 사용하여 학습을 시킨 것이지만, 이후에는 Transformer나 chromaGAN을 사용하여 프로젝트를 조금 더 발전시킬 생각이다. 
