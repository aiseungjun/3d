### 3D 렌더링과 모델링: 전통적 접근부터 NeRF까지

#### 1. 전통적인 3D 렌더링 방법
3D 렌더링은 3D 모델 데이터를 사용자가 보는 2D 화면으로 실시간으로 변환하는 과정입니다. 이 과정을 간단히 이해할 수 있는 영상들은 다음과 같습니다:

- [실감나는 3D 영상이 만들어지는 과정, 그래픽카드 속 렌더링 파이프라인의 원리](https://www.youtube.com/watch?v=BMT0xCxP6w8)  
- [How do Video Game Graphics Work?](https://www.youtube.com/watch?v=C8YtdC8mxTU&t=71s)  
- [The Math behind (most) 3D games - Perspective Projection](https://www.youtube.com/watch?v=U0_ONQQ5ZNM)  

이 영상들은 렌더링 파이프라인의 기본 과정을 설명하며, 3D 데이터를 2D 화면으로 변환하는 과정을 간단히 소개합니다. 그러나, 실제로는 다양한 렌더링 기술이 존재하며, 사용 목적에 따라 적절한 방법이 선택됩니다.

#### 2. 3D 모델링
3d 랜더링이 3d 모델 data에서 특정 시각에 대한 2d 화면을 보여주는 과정이라면, 이전에 3d 모델 data를 만드는 과정이 있고, 이를 3d 모델링이라 합니다. 즉, 3D 모델링은 3D 데이터를 생성하는 과정으로, 렌더링 이전 단계입니다. 대표적인 방법은 다음과 같습니다:

- [What Is Photogrammetry?](https://blogs.nvidia.com/blog/what-is-photogrammetry/)  
- [SFM (Structure From Motion) blog](https://mvje.tistory.com/92)  
- [Structure from Motion: 2D 이미지는 어떻게 3D로 재구성될까?](https://www.youtube.com/watch?v=LBW7a2UkRJI)  
- [사진 측량(Photogrammetry)과 LiDAR 측량의 차이 그리고 결합 방법](https://www.t3solution.co.kr/24/?bmode=view&idx=94757685)  

보통 이 중 몇 가지 매소드를 결합하여 모델링을 수행하는 것이 일반적이며, 애니메이션이나 영화에서는 이에 더해 그래픽 전문가들이 직접 polygon을 섬세하게 다듬으며 가상세계에서의 3D 모델에서 많이 작업합니다. 실시간 랜더링이 필요한 경우에는 polygon 개수를 줄이는 등의 방법으로, 디테일한 비실시간과 같은 영화에서는 섬세를 더 챙기는 trade-off가 존재합니다.

#### 3. 카메라 파라미터의 이해
마지막 traditional 3d CV 소개로, 3D 모델링과 렌더링을 이해하려면 카메라의 **Intrinsic/Extrinsic Parameters**에 대한 개념이 필수적입니다. 이를 쉽게 설명한 자료는 다음과 같습니다:

- [Camera Intrinsic/Extrinsic Parameters #1](https://xoft.tistory.com/12)  
- [Camera Intrinsic/Extrinsic Parameters #2](https://xoft.tistory.com/12)  
- [Matrices and Transformations - Math for Gamedev](https://www.youtube.com/watch?v=HgQzOmnBGCo)  

#### 4. NeRF: Neural Radiance Fields
NeRF는 3d 모델 랜더링의 과정에서 메모리를 최대 몇 천배 절약하며, 연속적으로 미분 가능한 ray based로부터 착안한 deep learning model이다. 다른 딥러닝 모델들과 가장 큰 차이점은, 이 모델은 overfitting이 되는 것이 목적입니다. 하나의 3d 모델 데이터에 최대한으로 loss를 줄여서 하나의 장면에 대한 3d 표현을 학습하는 것입니다. 
아래 블로그에서는 아주 디테일하게 NeRF-base 모델이 어떻게 아키텍처를 구성하고 훈련되었으며, 어떠한 한계점들이 있는지에 대해서 설명해줍니다. 
또 주목할 점은, 우선 nerf에서는 직접적으로 3d 모델 data를 output하는 대신 이를 딥러닝 모델에서 intrinsic 하게 학습을 하고, 바로 화면을 랜더링해낸다는 것입니다. 이러한 모델의 파라미터에 3d 모델 데이터를 학습시킴으로써 메모리는 최대 첫 천배 덜 사용하게 되고, 이를 심지어 continuous하게 접근할 수 있다는 것이 Nerf의 최대 novelty 중 하나일 것입니다. 그러나 다양한 각도의 많은 사진 & pose 데이터 필수, inference 속도 느림 등 다양한 단점들이 있고, 이에 대해서도 잘 고민해보아야 합니다.

- [NeRF Blog](https://nuggy875.tistory.com/168)  

#### 5. 3D Gaussian Splatting: 새로운 접근
NeRF와는 완전히 다른 접근법으로, **3D Gaussian Splatting** 모델이 주목받고 있습니다. Traditional CV based 접근으로 시작해서, 층층히 모델을 쌓는 대신 3d 모델의 부분부분의 성분들을 직접 학습해내겠다는 접근입니다. 관련 자료는 다음에서 확인할 수 있습니다:

- [3D Gaussian Splatting Blog](https://xoft.tistory.com/51)  
- [Getting Started With 3D Gaussian Splatting for Windows (Beginner Tutorial)](https://www.youtube.com/watch?v=UXtuigy_wYc&t=2094s)  
