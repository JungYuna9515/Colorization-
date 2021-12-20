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
1번 실험(RGB to RGB)-Generator:Unet, Discriminator-PatchGAN(16x16)<br>
2번 실험(RGB to RGB)-Generator:Unet(w/o Dropout), Discriminator-PatchGAN(1x1)
3번 실험(RGB to RGB)-Generator:Unet(w/o Dropout), Discriminator-PatchGAN(8x8)
4번 실험(Lab to RGB)-Generator:Unet(w/o Dropout), Discriminator-PatchGAN(8x8)

## 결과
#### **1. RGB to RGB - 1번실험**<br>
![loss1](https://user-images.githubusercontent.com/65711055/146713838-7c9b4a5d-cd7c-4b8a-9b9b-83928d568b0f.png)<br>
![실험1 그림1](https://user-images.githubusercontent.com/65711055/146713850-812d6937-b94e-44c2-8571-94108219a989.png)<br>
ssim:  0.9968320418069206, psnr = 63.6979<br>
![실험1 그림2](https://user-images.githubusercontent.com/65711055/146713858-332fa3e9-3e3a-4526-a75e-3143a1669b46.png)<br>
ssim:  0.9997259981233829, psnr = 69.8225<br>
![실험1 그림3](https://user-images.githubusercontent.com/65711055/146713866-d27ee26c-2d8d-4595-99fb-c9ec7bc4dfb7.png)<br>
ssim:  0.9998020751613257, psnr = 67.9700<br><br><br>
#### **2. RGB to RGB - 2번실험**<br>
![loss2](https://user-images.githubusercontent.com/65711055/146714624-76205361-3f66-44c4-8257-78ff578968f3.png)<br>
![실험2 그림1](https://user-images.githubusercontent.com/65711055/146714636-c1ac6dbe-1486-4a60-8d3a-2f9d0883a7d5.png)<br>
ssim:  0.9999025335420327, psnr = 76.4366<br>
![실험2 그림2](https://user-images.githubusercontent.com/65711055/146714644-fbfcfba1-42fa-421d-a3b6-ca1a02289342.png)<br>
ssim:  0.9999220159364776, psnr = 74.1495<br>
![실험2그림3](https://user-images.githubusercontent.com/65711055/146714678-256ba3d6-ffab-4dd1-adff-66c92223b561.png)<br>
ssim:  0.9997991687728733 psnr =  69.5486<br><br><br>
#### **3. RGB to RGB - 3번실험**<br>
![loss3](https://user-images.githubusercontent.com/65711055/146714903-e88e38c6-e5e8-4ebd-b8e9-0c8aa831bbf4.png)<br>
![실험3 그림1](https://user-images.githubusercontent.com/65711055/146714911-24577252-f524-419a-be3a-13ace25fb15e.png)<br>
ssim:  0.9996700831853466, psnr = 76.1649<br>
![실험3 그림2](https://user-images.githubusercontent.com/65711055/146714927-ac61beb8-3f13-45a7-a411-bc1a13d54ed2.png)<br>
ssim:  0.999896599125892, psnr = 73.4646<br>
![실험3 그림3](https://user-images.githubusercontent.com/65711055/146714941-5d77a390-9635-4110-8a59-f6bfed70e36a.png)<br>
ssim:  0.9993893211720509 psnr = 72.2418<br><br><br>
**4. Lab to RGB - 4번실험**<br>
![loss4](https://user-images.githubusercontent.com/65711055/146715478-f822f386-aa43-4314-81b4-7dbb187be7d9.png)<br>
![실험4 그림1](https://user-images.githubusercontent.com/65711055/146715488-764c8deb-8547-4cec-af6c-2be74e9a8aab.png)<br>
ssim:  0.999946547847351 psnr =  78.69126<br>
![실험4 그림2](https://user-images.githubusercontent.com/65711055/146715502-cffe3ca3-3749-4787-9bf5-07230a84d39d.png)<br>
ssim:  0.9999286993021637 psnr =  74.974174<br>
![실험4 그림3](https://user-images.githubusercontent.com/65711055/146715521-96fd179f-44b4-48d8-aab9-3c0b2cb2e712.png)<br>
ssim:  0.9999492215021117 psnr =  78.76771<br><br><br>
## Conclusion
원래는 Colorizaiton을 위해 나온 모델인 ‘chromaGAN’까지 사용하는 것이 목표였는데, 처음 GAN을 사용하는 것이다 보니 미숙했던 점도 있었고, 색공간을 바꾸어가면서 시각화를 진행하려고 하다 보니 이 부분이 막혀서 시간이 많이 지체되어 목표를 달성하진 못했다. 하지만 pix2pix 모델을 사용하여 결과를 내기 위해 모델도 다양하게 바꾸어보고, activation function, 하이퍼파라미터, 전처리 방식도 다양하게 바꾸어가면서 사용해볼 수 있었기 때문에 더 깊이있게 이해할 수 있었던 것 같다. 특정한 부분에서 시간을 많이 잡아먹은 것은 아쉽지만, 이러한 경험을 통해서 앞으로 동일한 상황에 직면했을 때 문제를 더 효율적으로 해결할 수 있을 것 같다. 이 프로젝트는 GAN을 사용하여 학습을 시킨 것이지만, 이후에는 Transformer나 chromaGAN을 사용하여 프로젝트를 조금 더 발전시킬 생각이다. 
