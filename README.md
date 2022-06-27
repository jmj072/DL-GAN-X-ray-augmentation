# GAN을 이용한 X-ray 이미지 증강

## 프로젝트 진행 배경

- 의료영상 데이터는 환자 데이터가 정상 데이터 보다 적은 경우가 많으므로, 데이터 불균형을 피하기 어려움.  
- 간단한 의료영상인 X-ray 이미지를 이용해, 실제로 데이터 불균형을 해결하는지 확인할 수 있음
  
> ☑ GAN을 이용해 Fake 환자 데이터를 생성하여 데이터 불균형을 해결하고, 예측 모델의 성능을 향상시키고자 함

<br>

## 개발 환경
- Google Colab
- 사용된 library & Tools
  - `pandas`, `numpy`, `matplotlib`, `tensorflow`, `keras`

## 파일 구성
  - `1_Dataset`
    - `1_1_Before_GAN`
      -  데이터 증강 이전 데이터셋
    - `1_2_After_GAN`
      - GAN 모델로 Covid-19 환자 데이터를 증강후 추가된 데이터셋
  - `2_Model`
    - `2_1_before_GAN.pkl`
      - 데이터 증강 이전 CNN 모델
    - `2_2_GAN_EPOCH_1000.pkl`
      - Covid-19 환자 X-ray 이미지를 Epoch 1000번 시행한 DCGAN 모델
    - `2_3_after_GAN.pkl`
      - 데이터 증강 후 CNN 모델

  - `3_images`
    - GAN에서 생성한 결과 이미지를 epoch에 따라 저장

  - `4_Before_GAN_training.ipynb`
    - 데이터 증강 전 CNN 모델 학습
  - `5_GAN_training.ipynb`
    - GAN model 학습
  - `6_After_GAN_traing.ipynb`
    - 데이터 증강 후 CNN 모델 학습

<br>

## Pipeline
![image](https://user-images.githubusercontent.com/77204538/166177996-51fc7ccf-6ebb-43a6-a285-3dc603689ff8.png)

<br>


## 1. Dataset
- Kaggle의 “Chest X-ray (Covid-19 & Pneumonia) Dataset”을 사용[(데이터셋 링크)](https://www.kaggle.com/djibybalde/dcgan-keras-chest-x-ray-images)
- 정상, 코로나19환자, 폐렴환자로 구성 되어있으나, __정상, 코로나19 환자의 데이터만 사용하였음__
  - Train set ( 정상군:1266명, 코로나19환자:460명)
  - Test set (정상군:317명, 코로나19환자:116명)

<br>

## 2. 데이터 전처리
- 이미지 크기를 128 x 128 로 축소
- 이미지 정규화 (0~1사이의 값)

<br>

## 3. Modeling
### 3.1 GAN 모델 학습
- DCGAN(Deep Convolutional Generative Adversarial Network) 모델 사용
- Epoch 1000번 시행
- 코로나19 환자의 Fake 이미지를 1000장 생성하여 기존의 데이터에 더함
- 생성 결과 이미지

  ![image](https://user-images.githubusercontent.com/77204538/166178892-5c2f7e9a-1edd-428a-8b3e-3d35d032980e.png)


<br>

## 3.2 CNN 학습
- CNN 모델 구조

  ![image](https://user-images.githubusercontent.com/77204538/166178321-88453258-5704-42e6-a6c4-a1f750364038.png)
  
- 이진 분류 모델 생성 (정상군, 코로나19 환자군)
- Early stop을 이용해 최대한 학습
- 데이터 증강 전 CNN 모델 : `Before_GAN_training.ipynb` -> `before_GAN.pkl` 생성
- 데이터 증강 후 CNN 모델 : `After_GAN_traing.ipynb` -> `after_GAN.pkl` 생성

<br>

- Fake Data 추가 후, 모델 성능 비교

  ![image](https://user-images.githubusercontent.com/77204538/166179042-cdd394fd-81f4-4588-90a9-6f181b0ab8b1.png)

  - 미세한 성능향상이 확인됨

<br>

## 4. 결론
> 환자의 Fake 이미지를 Training Dataset에 추가할 경우, 분류 모델의 성능이 향상한다.

<br>

## 5. 한계점 및 개선사항
### 한계점
- Fake Dataset을 추가하였을 때, 미세한 성능향상을 보임
  - 간단한 이진 분류 과제였으므로, 추가하기 이전의 분류 모델의 성능이 매우 좋았음
- Fake 이미지의 추가가 단순 증강(Filp, Rotaion 등)과 비교해 효과가 있는지 알 수 없음
- Fake 이미지가 예측에 어떤 영향을 주는지 알 수 없음
  - Fake 이미지를 Training Set에만 추가하여, Fake 이미지와 실제 이미지의 분류 정확도를 비교할 수 없음

### 개선사항
  - 단순 이진 분류가 아닌, 다른 그룹(폐렴 환자 등)을 추가하여 성능을 살펴볼 것
  - 코로나19 환자 이미지를 Translation, Flip, Rotation 하여 Fake 이미지와 동일한 수로 데이터를 증강하여 분류 모델의 성능을 비교해볼 것
    - GAN 모델 사용이 기존 방법보다 효과가 있는지?
  - 생성된 Fake 이미지를 Test Set에도 섞어볼 것

<br>

## 후기
- 석사과정 당시에 환자 데이터가 너무 적어서 통계 분석에 어려움을 겪었던 적이 있어 프로젝트를 기획했지만, 너무 단순한 과제여서 그런지 데이터 증강의 효과를 명확하게 보지못해 너무 아쉬웠다.  

- 부트캠프에서 처음 GAN을 배운 것이었지만, 활용해보면서 가짜 이미지를 상당히 잘 만드는 걸 느꼈다. 현미경으로 촬영한 세포 이미지와 같이 구하기 어려운 이미지에 활용하기에 참 좋은 모델일 것 같다. (찾아보니 실제로 생물학적 이미지에 활용하는 경우도 있었다.)




[[프로젝트 상세] 발표 PPT File 링크](https://drive.google.com/file/d/1ydedY4Mka50gqSV0BWeCwzuCpkmydjuz/view?usp=sharing)
