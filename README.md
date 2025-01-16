## 저화질 CCTV 차량 번호판 이미지 Super-Resolution 
### Super-Resolution for Low-Resolution CCTV Vehicle License Plate Images


## Abstract
License plate recognition from CCTV footage can vary in accuracy depending on the image quality. To address this issue, we introduce a method that utilizes Super-Resolution techniques to reconstruct low-quality license plate images into high-quality ones.

## Keywords

- SRCNN
- VDSR
- RCAN


## Experiments
#### SRCNN(Super-Resolution Convolutional Neural Network)
Super-Resolution Convolutional Neural Network(SRCNN)은 단일 이미지 초해상화 문제를 해결하기 위해 처음으로 딥러닝 기반 접근 방식을 도입한 모델이다. 기존의 예제 기반 초해상화 기법은 사전 처리 및 사후 처리가 요구되는 복잡한 파이프라인을 사용했으나, SRCNN은 저해상도(LR) 이미지를 고해상도(HR) 이미지로 직접 변환하는 단순한 엔드투엔드 학습 프레임워크를 제안하였다.

SRCNN은 3개의 컨볼루션 레이어로 구성된 비교적 간단한 네트워크 구조를 채택하여, 기존 방법론 대비 학습 효율성과 계산 속도를 크게 향상시켰다. 또한, 비선형 매핑 및 재구성(Reconstruction) 단계를 포함한 네트워크의 구성은 고주파 정보를 효과적으로 학습할 수 있도록 설계되었다.

그러나 SRCNN은 얕은 네트워크 구조로 인해 복잡한 고주파 세부 정보를 학습하는 데 한계를 보였다. 이러한 한계를 극복하기 위해 VDSR과 같은 보다 심층적인 네트워크 구조가 제안되었다.

![SRCNN](https://github.com/user-attachments/assets/67a95a14-12a5-4f93-9053-c5d1971be41a)

#### VDSR(Very Deep Super-Resolution)
Very Deep Super-Resolution Network(VDSR)는 SRCNN의 한계를 극복하기 위해 제안된 모델로, 깊은 네트워크 구조를 통해 고해상도 이미지를 더욱 정밀하게 복원하도록 설계되었다. VDSR은 20개의 컨볼루션 레이어를 활용하여 SRCNN보다 훨씬 넓은 수용 영역(Receptive Field)을 확보하였으며, 더 많은 컨텍스트 정보와 복잡한 이미지 특징을 효과적으로 학습할 수 있다.

또한, VDSR은 잔차 학습(Residual Learning) 기법을 활용하여 학습 효율성을 크게 개선하였다. 잔차 학습은 입력 이미지와 고해상도 이미지 간의 차이를 모델링함으로써 네트워크가 복잡한 세부 정보를 더 빠르고 정확하게 학습할 수 있도록 돕는다. 이는 그래디언트 소실 문제를 완화하고, 네트워크 깊이가 증가하더라도 안정적이고 효율적인 학습 과정을 가능하게 한다.

![VDSR](https://github.com/user-attachments/assets/ca48709c-8a11-45c7-930c-bc19fc319702)

#### RCAN(Residual Channel Attention Network)
Residual Channel Attention Network(RCAN)는 이미지 초해상도 문제를 해결하기 위해 설계된 심층 신경망으로, 고주파 정보를 담고 있는 중요한 채널에 가중치를 부여하여 학습 성능을 극대화하는 모델이다. RCAN은 네트워크의 깊이를 크게 확장하면서도 학습 안정성을 유지하기 위해 Residual in Residual(RIR) 구조와 Channel Attention(CA) 메커니즘을 결합한 것이 특징이다.

RCAN은 입력 이미지를 얕은 특징 맵으로 변환한 후 RIR 구조를 사용하여 깊은 특징을 추출한다. RIR은 여러 개의 Residual Group(RG)과 롱 스킵 연결(Long Skip Connection)로 구성된다. 각 RG는 다수의 Residual Channel Attention Block(RCAB)으로 이루어져 있으며, RCAB는 Conv 레이어와 숏 스킵 연결(Short Skip Connection)을 통해 잔차를 학습한다. 이러한 구조는 고주파 정보를 강조하며 네트워크가 중요한 정보에 집중하도록 설계되었다.

RCAB 내부의 채널 주의 메커니즘은 글로벌 평균 풀링(Global Average Pooling)을 통해 각 채널의 전역 정보를 요약한 후, 두 개의 Conv 레이어와 Sigmoid 활성화 함수를 사용하여 채널별 중요도를 계산한다. 이 과정을 통해 중요한 정보를 더욱 강조하며, 최종적으로 고해상도로 변환된 이미지를 출력한다.

![RCAN](https://github.com/user-attachments/assets/048b0e08-a425-4bbc-82fc-501893050702)

## Results

![결과이미지](https://github.com/user-attachments/assets/564ce706-64bc-4a05-9140-d36c5ffff195)
![loss 이미지](https://github.com/user-attachments/assets/de841487-91d8-45cc-8b2a-eecda9bd6acd)
![평가지표](https://github.com/user-attachments/assets/ee6fb69a-f545-43f4-86c2-1f9c774281f3)


## References
- [1] Lim YJ. Nationwide public CCTV 45% 'outdated'... Faces unrecognizable even when criminals are captured. YTN. October 9, 2023. Available from: https://www.ytn.co.kr/_ln/0103_202310090814140873
- [2] Wang Z, Chen J, Hoi SCH. Deep learning for image super-resolution: A survey. IEEE Trans Pattern Anal Mach Intell. 2021;43(10):3365-3387. doi:10.1109/TPAMI.2020.2982166.
- [3] Dong C, Loy CC, He K, Tang X. Image super-resolution using deep convolutional networks. IEEE Trans Pattern Anal Mach Intell. 2016;38(2):295-307.
- [4] Kim J, Lee JK, Lee KM. Accurate image super-resolution using very deep convolutional networks. IEEE Conf Comput Vis Pattern Recognit. 2016;1646-1654. doi:10.48550/arXiv.1511.04587.
- [5] Zhang Y, Li K, Li K, Wang L, Zhong B, Fu Y. Image super-resolution using very deep residual channel attention networks. Eur Conf Comput Vis. 2018;286-301. doi:10.48550/arXiv.1807.02758.
- [6] Horé A, Ziou D. Image quality metrics: PSNR vs. SSIM. Proc 20th Int Conf Pattern Recognit. 2010:23-26 Aug; Istanbul, Turkey. IEEE; 2010. doi:10.1109/ICPR.2010.579.
