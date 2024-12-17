# Enhancing Vehicle License Plate Quality Using Super-Resolution

## Abstract
License plate recognition from CCTV footage can vary in accuracy depending on the image quality. To address this issue, we introduce a method that utilizes Super-Resolution techniques to reconstruct low-quality license plate images into high-quality ones.

## Keywords
- License Plate Recognition
- Super-Resolution(SR)
- SRCNN
- VDSR
- Deep Learning

## introduction



## Experiments

데이터 수집 및 전처리 
본 연구에서는 AI 인프라를 지원하는 통합 플랫폼인 AI-Hub에서 자동차 차종, 연식, 번호판 인식을 위한 영상을 다운로드하여 데이터셋으로 활용하였다. 자동차 번호판 이미지 데이터셋의 시각화는 <그림>에 나타나 있다. 전체 데이터는 70,000개의 Train 데이터, 7,000개의 Validation 데이터, 3,000개의 Test 데이터로 나누어 구성하였다. 

![1](https://github.com/user-attachments/assets/46e06b59-ba25-4c16-ae3c-27bd9224c469)

현실에서는 저화질 이미지 데이터를 활용하여 Super-Resolution을 수행해야 하지만, 저해상도(LR) 이미지 만으로는 해당 문제를 효과적으로 해결하기 어렵다. 따라서, 해당 문제를 지도학습 방식으로 접근하기 위해 원본 자동차 번호판 이미지를 저해상도(LR) 이미지로 변환하는 Degradation 과정을 거쳐 사전 전처리를 수행해야 한다.

![수식](https://github.com/user-attachments/assets/275ca8a6-97f5-42d2-96d3-9d6df45bf44a)

Degradation 문제는 <수식 1>과 같이 정의된다.  본 연구에서는 현재 연구 동향을 참고하여 표준화된 Degradation 방식인 Bicubic 다운샘플링을 적용하였다. 비록 해당 방식이 현실에서 발생하는 저해상도(LR) 이미지를 완벽히 대변할 수는 없지만, 자연스러운 블러링 효과와 디테일 손실을 포함하는 특성을 가지고 있어 해당 연구의 Degradation에 적합하다고 판단하였다.


<그림>에는 Bicubic 다운샘플링 이미지 전처리를 통해 저해상도(LR) 이미지를 확인할 수 있다.

![2](https://github.com/user-attachments/assets/dc8da6a1-c180-479a-b2e7-6f957b637905)

모델 활용 및 설계
딥러닝 기반 Super-Resolution(SR) 분야의 시초 모델인 SRCNN과 잔차학습을 활용해 깊은 네트워크 구조로 세밀한 특징을 학습하고 픽셀 복원을 하는 VDSR 모델을 활용해 저화질 자동차 번호판 이미지를 개선하고자 한다.

![3](https://github.com/user-attachments/assets/7d0dfb1c-de32-4eb5-8a92-3c98b3bcb396)

<그림>을 보면 해당 모델 구조를 확인할 수 있다. SRCNN 모델은 3채널의 저해상도(LR) 이미지를 입력으로 받아, 세 개의 컨볼루션 레이어를 거쳐 고해상도(HR) 이미지로 변환한다. 첫 번째 컨볼루션 레이어에서는 9x9 필터를 사용하여 입력 이미지를 연산한 후, 패딩 4를 적용하여 이미지 크기를 유지하며, 64개의 특징맵으로 변환한다. 이 과정에서 활성화 함수로 ReLU를 사용하여, 저차원 공간에서 유의미한 특징을 추출하고 두 번째 컨볼루션 레이어로 전달한다.
두 번째 컨볼루션 레이어는 5x5 필터를 사용하여 64개의 특징맵을 32개로 축소하며, 패딩 2를 적용하여 크기를 유지한다. 이 레이어는 비선형 활성화 함수 ReLU를 통해 저해상도 특징을 고해상도 특징으로 매핑하는 역할을 한다.
마지막 세 번째 컨볼루션 레이어에서는 5x5 필터를 사용하여 32개의 특징맵을 3채널(RGB)의 고해상도 이미지로 변환하며, 패딩 2를 적용해 이미지 크기를 유지한다. 이렇게 학습된 결과를 기반으로 고해상도 이미지를 재구성하여 최종 출력으로 제공한다.

![4](https://github.com/user-attachments/assets/d1381120-14bb-43a8-89bd-3d609fa54787)

SRCNN의 단점은 3개의 컨볼루션 레이어만 사용하여 네트워크가 매우 얕고, 세부적인 특징과 패턴을 학습하는 데 한계가 있다는 점이다. VDSR은 이러한 단점을 극복하기 위해 네트워크를 20개의 레이어로 확장하여 입력 이미지에서 더 복잡한 특징을 학습할 수 있도록 설계되었으며, 잔차 학습(residual learning)을 도입함으로써 저화질 이미지를 고화질 이미지로 재구성하는 효율성을 향상시켰다.

첫 번째 컨볼루션 레이어는 저화질(LR) 이미지의 RGB 3채널을 입력으로 받아, 3x3 크기의 커널 필터를 사용하여 64개의 특징맵을 생성한다. 이 과정에서 패딩 1을 적용해 이미지 크기를 유지하며, 활성화 함수로 ReLU를 사용한다.
이후 18개의 잔차 블록(Residual Block)으로 구성된 컨볼루션 레이어를 통해 복잡한 특징을 학습한다. 각 블록은 3x3 커널 필터와 패딩 1을 적용하며, 활성화 함수로 ReLU를 사용하여 고주파 정보(에지, 텍스처 등)를 효과적으로 복원할 수 있도록 네트워크를 설계하였다.

마지막 컨볼루션 레이어에서는 64개의 채널을 3채널 RGB로 변환하기 위해 3x3 커널 필터를 사용하며, 패딩 1을 적용하여 크기를 유지한다. 최종적으로 입력 이미지를 네트워크 출력에 더해, 고해상도(HR) 이미지를 재구성한다.


학습 시에 이미지 크기는 256x256으로 Resize, Batch Size 16, Optimizer는 Adam, Learning rate 0.0001로 설정하여 epoch 25번 반복 학습하도록 하였다.

![5](https://github.com/user-attachments/assets/88f06bcc-094c-4be5-9df6-c9ea6ea6d7b5)



## Results

![4](https://github.com/user-attachments/assets/da5f344d-dd8b-491c-a815-19f1ae452fa4)
![out1](https://github.com/user-attachments/assets/20e68b58-9e8d-4033-a0d4-2f733360e9a2)
![out2](https://github.com/user-attachments/assets/b8f7497e-6f93-44fa-934f-608b183e1361)
![out3](https://github.com/user-attachments/assets/5d27c5b6-fde3-4b88-bc9d-337dacbe7448)
![out4](https://github.com/user-attachments/assets/40d8ec12-c5be-4f14-89e3-7acf330ea576)
![output1](https://github.com/user-attachments/assets/7a05860a-8559-4294-84ab-7c6210294c25)
![결과1](https://github.com/user-attachments/assets/bb9a6a62-9f63-4bcd-8c1b-20ad01db1d31)
![결과2](https://github.com/user-attachments/assets/4425e8b1-0bfd-4573-b66c-e5832aed406d)
![결과3](https://github.com/user-attachments/assets/929aa060-f15e-4e3e-b7fe-5bfeb0891be7)
![결과4](https://github.com/user-attachments/assets/d08c6d79-3c18-4282-9f38-337e4a20e101)
![결과5](https://github.com/user-attachments/assets/b2de7f63-6ece-4e01-bdc6-989974c77f66)
![결과6](https://github.com/user-attachments/assets/934fbcab-740d-464f-842d-012dc2362be5)

![평가지표](https://github.com/user-attachments/assets/f3f9d6a0-cfb0-4bf3-bb29-0f0bd8302091)
![결과차트](https://github.com/user-attachments/assets/8e8cf1a1-8e40-4f8f-ba48-d0defffaa49a)
## Discussion

## Conclusion

## References
