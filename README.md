# 2022-Anomaly-Detection-using-GAN

## 🚩 주제

**AnoGAN**과 **GANomaly**는 실제와 유사한 이미지를 생성하는 GAN 모델을 변형하여, 이미지 이상치 탐지에 이용한 모델이다. 논문을 통해 **모델의 구조를 이해하고 구현**하여,
**주형의 결함을 탐지**하는 데에 적용하였다.
<br> </br>

## 🔎 AnoGAN 모델 구조

AnoGAN은 크게 2단계로 나눌 수 있다. 첫 번째 단계는, DCGAN과 동일한 구조의 모델을 정상 이미지로만 학습시킨다. 정상 데이터만 사용할 뿐, 모델의 모든 구조와 손실 함수는 DCGAN과 동일하다. 두 번째 단계에서는 새로운 이미지를 기존의 latent space에 매핑시킨다. 새로운 이미지에 가장 적합한 latent variable z를 찾는 과정이다. 앞서 학습시킨 DCGAN의 G와 D의 모든 파라미터를 고정한 후, 랜덤으로 발생시킨 z만을 업데이트하는 것이다.

**즉, 정상 이미지만을 학습한 후, 새로운 이미지가 들어올 때마다 유사한 이미지를 생성시켜, 이미지가 제대로 생성되지 않으면 ‘이상치’로 탐지하는 모델이다.**

단점은, 새로운 이미지와 유사한 이미지를 생성해내는 Step 2를 모든 데이터에 대해 진행해야 하므로, 모델을 사용할 때에도 학습 과정이 필요해 많은 시간이 소요된다.
<br> </br>

## 🔎 GANomaly 모델 구조

GANomaly는 크게 3개의 subnetwork로 나뉜다. 입력과 비슷한 이미지를 생성해내는 Generator와 이를 다시 압축하는 Encoder, 실제와 가짜 이미지를 분류하는 Discriminator이다.

이 과정을 **정상 이미지로만 학습**하여, 정상 이미지를 압축했다가 비슷한 이미지로 재생산하는 모델을 학습시킨다. **이 모델에 이상치 이미지를 넣으면, 이미지가 제대로 생산되지 않아, 재생산된 이미지는 실제 이미지와 유사하지 않게 되고, 이 유사도를 기준을 ‘이상치’를 탐지한다.**

**정상 이미지를 압축하는 과정부터 학습하기 때문에, 새로운 이미지에 대해서는 유사도만 계산하기 때문에, AnoGAN에 비해 훨씬 짧은 시간이 소요된다.**
<br> </br>

## 📰 데이터
1. **Cloud and non-cloud data**

위성으로 지면을 촬영한 데이터로, 구름이 껴 있으면 지면을 제대로 촬영할 수 없어, cloud data를 이상치로 하는 데이터이다. 모양보다는 해상도를 잡아내는 것이 중요하다.

- 학습 데이터 : 정상 이미지 1400개
- 검증 데이터 : 정상 이미지 100개, 이상치 이미지 100개

2. **Casting data**

금속 공정에 사용되는 주형 이미지 데이터로, 여기에 금속 액상을 부어 굳히므로 주형의 결함은 곧 주조 결함으로 이어진다. 대부분 비슷하게 생겼기 때문에, 미세한 이상치를 잡아내야 한다.

- 학습 데이터 : 정상 이미지 2875개
- 검증 데이터 : 정상 이미지 100개, 이상치 이미지 100개
<br> </br>

## 💡 결과
- AnoGAN보다 GANomaly의 성능이 우수
- 생성된 이미지를 통해 계산한, 이상치 데이터의 residual 이미지를 통해 결함 파악 가능
- threshold에 따라 90% 이상의 정확도
<br>

## Reference Paper
📃 Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., Courville, A., Bengio, Y.: Generative adversarial nets. In: Advances in Neural Information Processing Systems. (2014)

📃 Radford, A., Metz, L., Chintala, S.: Unsupervised representation learning with deep convolutional generative adversarial networks. arXiv:1511.06434 (2015)

📃 Schlegl, T., Seebock, P., Waldstein, S.M., Schmidt-Erfurth, U., Langs, G.: Unsupervised anomaly detection with generative adversarial networks to guide marker discovery. arXiv:1703.05921 (2017). 

📃 Samet Akcay, Amir Atapour-Abarghouei, Toby P. Breckon: GANomaly: Semi-Supervised Anomaly Detection via Adversarial Training. 	arXiv:1805.06725 (2018)


### 데이터 출처
📰 Cloud and Non-Cloud Images (Anomaly Detection). https://www.kaggle.com/datasets/ashoksrinivas/cloud-anomaly-detection-images

📰 Casting product image data for quality inspection. https://www.kaggle.com/datasets/ravirajsinh45/real-life-industrial-dataset-of-casting-product

### Code 참고
https://github.com/soomin9106/Deep-Learning/blob/main/AnoGAN/DCGAN_ AnoGAN.ipynb <br>
https://github.com/leafinity/keras_ganomaly/blob/master/ganomaly.ipynb

