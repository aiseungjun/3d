nerf 이전에, 또한 현재에도 주를 이루는 tranditional 랜더링 매소드들을 가볍게 소개하는 영상들은 다음이 있다; <a herf='https://www.youtube.com/watch?v=BMT0xCxP6w8'>실감나는 3D 영상이 만들어지는 과정, 그래픽카드 속 렌더링 파이프라인의 원리</a>, <a herf='https://www.youtube.com/watch?v=C8YtdC8mxTU&t=71s'>How do Video Game Graphics Work?</a>, <a herf='https://www.youtube.com/watch?v=U0_ONQQ5ZNM'>The Math behind (most) 3D games - Perspective Projection</a>. 이 영상들은 3d 모델 데이터를 어떻게 사용자가 보는 2D 화면으로 실시간 랜더링을 하는지에 대한 과정을 간단하게 소개해준다. 그러나 이것만이 전부가 아니라, 훨씬 다양하고 복잡한 랜더링 방법론들이 있으며, 이는 상황에 따라 적용된다.   
반면, 3d 랜더링이 3d 모델 data에서 특정 시각에 대한 2d 화면을 보여주는 과정이라면, 이전에 3d 모델 data를 만드는 과정이 있고, 이를 3d 모델링이라 한다. 3d 모델링 또한 적용 분야에 따라 매우 다양한 방법으로 이루어진다. 이 중 주요 매소드를 보자면, photogrammetry(<a herf='https://blogs.nvidia.com/blog/what-is-photogrammetry/'>What Is Photogrammetry?</a>)와 같은 전통적인 기법, 이와 비슷한 traditional CV tasks인 SFM(<a herf='https://mvje.tistory.com/92'>SFM (Structure From Motion) blog</a>, <a herf='https://www.youtube.com/watch?v=LBW7a2UkRJI'>Structure from Motion : 2D 이미지는 어떻게 3D로 재구성될까?</a>)같은 방식이 있으며, 좀 더 비싼 point cloud를 생성하는 LiDAR (<a herf='https://www.t3solution.co.kr/24/?bmode=view&idx=94757685'>사진 측량(Photogrammetry)과 LiDAR 측량의 차이 그리고 결합 방법</a>) 등이 있습니다. 보통 이 중 몇 가지 매소드를 결합하여 모델링을 수행하는 것이 일반적이며, 애니메이션이나 영화에서는 이에 더해 그래픽 전문가들이 직접 polygon을 섬세하게 다듬으며 가상세계에서의 3D 모델에서 많이 작업한다. 실시간 랜더링이 필요한 경우에는 polygon 개수를 줄이는 등의 방법으로, 디테일한 비실시간과 같은 영화에서는 섬세를 더 챙기는 trade-off가 존재한다. 마지막으로, traditional CV 중에서 단연코 Camera Intrinsic/Extrinsic Parameters에 대한 이해는 필수로 하고 이후의 3d 랜더링에 대해 공부해야 한다. 이를 쉽게 설명한 블로그와 2d에서 잘 표현한 유튜브는 다음과 같다; <a herf=' Camera Intrinsic/Extrinsic Parameters #1'>Camera Intrinsic/Extrinsic Parameters #1</a>, <a herf='https://xoft.tistory.com/12'>Camera Intrinsic/Extrinsic Parameters #2</a>, <a herf='https://www.youtube.com/watch?v=HgQzOmnBGCo'>Matrices and Transformations - Math for Gamedev</a>.

이제 nerf를 이야기할 차례이다. nerf는 이러한 3d 모델 랜더링의 과정에서 메모리를 최대 몇 천배 절약하며, 연속적으로 미분 가능한 ray based로부터 착안한 deep learning model이다. 다른 딥러닝 모델들과 가장 큰 차이점은, 이 모델은 overfitting이 되는 것이 목적이다. 하나의 3d 모델 데이터에 최대한으로 loss를 줄여서 하나의 장면에 대한 3d 표현을 학습하는 것이다. 이 블로그 (<a herf='https://nuggy875.tistory.com/168'>nerf blog</a>)는 아주 디테일하게 nerf-base 모델이 어떻게 아키텍처를 구성하고 훈련했으며, 어떠한 한계점들이 있는지에 대해서 설명해준다. (<a herf='https://nuggy875.tistory.com/168'>nerf blog</a> 이것도) 주목할 점은, 우선 nerf에서는 직접적으로 3d 모델 data를 output하는 대신 이를 딥러닝 모델에서 intrinsic 하게 학습을 하고, 바로 화면을 랜더링해낸다는 것이다. 이러한 모델의 파라미터에 3d 모델 데이터를 학습시킴으로써 메모리는 최대 첫 천배 덜 사용하게 되고, 이를 심지어 continuous하게 접근할 수 있다는 것이다. 그러나 다양한 각도의 많은 사진, pose값 있어야 함, inference 속도 느림 등 다양한 단점들이 있고, 위 블로그에서는 이에 대해서도 다룬다.

또한, nerf와는 아예 다르게 이 3d reconstruction에 접근한 deep learning model로 3D Gaussian Splatting 모델이 있다. 이는 현재 당신이 3d에 관심이 있다면 매우 주목할만 하다. traditional CV based 접근에, 층층히 모델을 쌓는 대신 3d 모델의 부분부분의 성분들을 직접 학습해내겠다는 접근이다. 잘 설명한 블로그는 다음과 같다; <a herf='https://xoft.tistory.com/51'> 3D Gaussian Splatting (SIGGRAPH 2023)</a> 코드 데모는 다음과 같다;<a herf='https://www.youtube.com/watch?v=UXtuigy_wYc&t=2094s'> Getting Started With 3D Gaussian Splatting for Windows (Beginner Tutorial)</a>