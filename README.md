# GAN을 이용한 X-ray 이미지 증강

- 의료영상 데이터는 환자 데이터가 정상 데이터 보다 적은 경우가 많으므로, 데이터 불균형을 피하기 어려움.  
- 이를 보완하기 위해, GAN을 이용해 Fake 환자 데이터를 생성하여 데이터 불균형을 해결하고, 모델의 성능을 향상시키고자 함.  
- 간단한 의료영상인 X-ray 이미지로 이를 시험해보고자 프로젝트를 진행하였음.
<br>

- 파일 구성
  - **Dataset**
    - `Before_GAN` : 데이터 증강 이전 데이터셋
    - `After_GAN` : GAN 모델로 Covid-19 환자 데이터를 증강후 추가된 데이터셋
  - **Model**
    - `before_GAN.pkl` : 데이터 증강 이전 CNN 모델
    - `GAN_EPOCH_1000.pkl` : Covid-19 환자 X-ray 이미지를 Epoch 1000번 시행한 DCGAN 모델
    - `after_GAN.pkl` : 데이터 증강 후 CNN 모델

  - **images** : GAN에서 생성한 결과 이미지를 epoch에 따라 저장

  - `Before_GAN_training.ipynb` : 데이터 증강 전 CNN 모델 학습
  - `GAN_training.ipynb` : GAN model 학습
  - `After_GAN_traing.ipynb` : 데이터 증강 후 CNN 모델 학습
---
<br>

## 전체 파이프라인
![image](https://user-images.githubusercontent.com/77204538/166177996-51fc7ccf-6ebb-43a6-a285-3dc603689ff8.png)

<br>


## Dataset
- Kaggle의 “Chest X-ray (Covid-19 & Pneumonia) Dataset”을 사용[(데이터셋 링크)](https://www.kaggle.com/djibybalde/dcgan-keras-chest-x-ray-images)
- 정상, 코로나19환자, 폐렴환자로 구성 되어있으나, __정상, 코로나19 환자의 데이터만 사용하였음__
  - Train set ( 정상군:1266명, 코로나19환자:460명)
  - Test set (정상군:317명, 코로나19환자:116명)

<br>

## 데이터 전처리
- 이미지 크기를 128 x 128 로 축소
- 이미지 정규화 (0~1사이의 값)

<br>

## GAN 모델 학습
- DCGAN(Deep Convolutional Generative Adversarial Network) 모델 사용
- Epoch 1000번 시행
- 코로나19 환자의 Fake 이미지를 1000장 생성하여 기존의 데이터에 더함
- 생성 결과 이미지

  ![image](https://user-images.githubusercontent.com/77204538/166178892-5c2f7e9a-1edd-428a-8b3e-3d35d032980e.png)


<br>

## CNN 학습
- CNN 모델 구조

  ![image](https://user-images.githubusercontent.com/77204538/166178321-88453258-5704-42e6-a6c4-a1f750364038.png)
  
- 이진 분류 모델 생성 (정상군, 코로나19 환자군)
- Early stop을 이용해 최대한 학습
- 데이터 증강 전 CNN 모델 : `Before_GAN_training.ipynb` -> `before_GAN.pkl` 생성
- 데이터 증강 후 CNN 모델 : `After_GAN_traing.ipynb` -> `after_GAN.pkl` 생성

<br>

## 성능 비교
![image](https://user-images.githubusercontent.com/77204538/166179042-cdd394fd-81f4-4588-90a9-6f181b0ab8b1.png)


