# DRRN : https://openaccess.thecvf.com/content_cvpr_2017/papers/Tai_Image_Super-Resolution_via_CVPR_2017_paper.pdf 

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Image Super-Resolution via Deep Recursive Residual Network
----------------------------------------------------------

* 좋은 성능에도 불구하고 많은 파라메터, 저장공간 필요
* 모바일 시스템에 적용이 잘 안됨
* Global Residual Learning(GRL)
* Local Residual Learning(LRL)
* Recursive Blocks
* 2x, 6x, 14x scale의 VDSR, DRCN, RedNet 보다 더 적은 파라메터 요구
* 더 깊고 간결한 Network로 성능 올림

DRRN의 2가지 주요 알고리즘
------------------------
* Both Global and Local residual learning
  * GRL(Global Residual Learning)
    * Input과 Output은 유사한 정보를 가지고 있음
    * 깊은 네트워크 학습의 어려움을 쉽고 효율적으로 만듦
  * LRL(Local Residual Learning)
    * 깊은 네트워크 통과시 많은 양의 Img 세부 정보 손실
    * 각각 단계 마다 정보를 전달할 수 있어 문제 해결
* Recursive Learning of Residual Units
  * 여러 개의 Residual Units으로 구성된 Recursive Block
  * Multi-Path LRL 구조로 Gradient 문제 해결
  
DRRN Architecture
-----------------
![image](https://user-images.githubusercontent.com/61686244/94770515-bbeef980-03ef-11eb-8c8d-c22ea58e31c8.png)
* Resnet, Resnet의 Residual unit을 사용하여 skip connection을 적용한 VDSR, skip connection과 Recursive 구조를 이용하여 추가적인 파라미터없이 성능을 올린 DRCN의 구조이고 각각의 Network의 장점들을 모아 사용한 제안된 DRRN의 구조 
* DRRN을 구조를 보게되면 Input과 Output을 연결해주는 Global residual learning, GRL과 Recursive Block, 그 안에 있는 Residual Units 그리고 Residual units 사이에서 적용하는 Local Residual learning, LRL을 볼 수 있음

Pre-activation Architecture
---------------------------
![image](https://user-images.githubusercontent.com/61686244/94770802-80086400-03f0-11eb-9c53-e6315d4bc6ce.png)
* 기존 Resnet을 개선하여 나온 Pre-Activation ResNet의 Pre activation structure를 사용
* ResNet팀은 (a)와 같이 기존의 오리지날 ResNet은 ReLU의 위치가 addition뒤에 위치해있는데 결과를 forward/backward propagation시킬때 약간의 blocking 구실을 했다고함
* 수학적인 조정을 통해 활성함수의 위치를 (e)와 같이 Residual net쪽에 위치를 시켜 수학적으로 완결된 형태를 만들고 이로 인해 결과를 더욱 개선하였다고함

Residual Unit & Recursive Block
-------------------------------
![image](https://user-images.githubusercontent.com/61686244/94771913-64eb2380-03f3-11eb-9566-495ddb9775fe.png)
![image](https://user-images.githubusercontent.com/61686244/94772038-b398bd80-03f3-11eb-8b90-5d07565c97bf.png)
* 최종 DRRN의 Network구조 
* B는 Recursive Block의 수를 의미하고 U는 residual 의 수를 의미
* 다음 그림의 예시 처럼 Recursive Block이 6개이고 각각의 Block마다 residual unit의 수를 3으로 지정했을 때의 그림
* B=6, U=3일때 깊이를 구하게 되면 총 43개의 층이 생김
* DRRN의 Network의 Loss Function은 Ground Truth., GT와 Network를 통해 나온 값의 차인 MSE를 사용 

PSNR of various DRRNs at B and U combinations
---------------------------------------------
![image](https://user-images.githubusercontent.com/61686244/94772095-d3c87c80-03f3-11eb-9e68-f5e9c3bd89f4.png)
* Training dataset으로는 Yang데이터 셋의 91개의 이미지와 BSD200 그래서 총 291개의 이미지를 Data Augmentation과 Scale Augmentation을 사용 하였고 Test dataset으로는  Set5, Set14, BSD100, Urban100을 사용 했고 각각의 Convolution은 3x3 커널을 가지는 128개의 필터를 가지고 있음
* Recursive Block의 수 B와 Residual Units의 수 U를 다양하게 조합하여 사용했을때의 결과
* B나 U의 파라미터를 3으로 고정하고 다른 파라메터를 1에서 4까지 변형시키며 비교
* 결과로는 B또는 U가 증가하며 Network의 깊이가 길어질 수록 가장 좋은 결과를 만들었다고 하고, 노란색 점인 B17U1, B3U8, B1U25는 조합은 50개의 층 이상을 의미하게 되며 B1U25가 가장 좋은 퍼포먼스를 냈음

Average PSNR/SSIM 
-----------------
![image](https://user-images.githubusercontent.com/61686244/94772568-decfdc80-03f4-11eb-88ea-68bb2686232d.png)
* DRRN B1U9모델은 VDSR과 DRCN과 Network의 깊이는 같고 더 적은 파라미터를 사용했는데 결과는 더 좋음
* DRRN B1U9모델의 깊이보다 더 깊게 만들어진 DRRN B1U25, 52개의 층을 가진 Network가 가장 좋은 성능을 만듦
* Urban100 테스트 데이터 셋에서는 다른 네트워크 들이 좋지 못한 성능을 냈지만 DRRN Network는 Urban100 dataset 에서도 좋은 성능을 만듦

Average IFCs
------------
![image](https://user-images.githubusercontent.com/61686244/94772728-3a9a6580-03f5-11eb-818f-1fac0633738c.png)
* 사람이 결과물을 시각적으로 봤을때 평가한 것으로, PSNR, SSIM이 아닌 사람이 본 시각적인 평가에서도 DRRN이 가장 좋은 성능을 만들었고, 20층이 B1U9 DRRN network에서 288x288 img를 처리하는데 0.25초걸림

Qualitative comparison
----------------------
![image](https://user-images.githubusercontent.com/61686244/94772768-5140bc80-03f5-11eb-98ed-68c8d47429db.png)

Number of parameters
--------------------
![image](https://user-images.githubusercontent.com/61686244/94772855-82b98800-03f5-11eb-9eb4-04dd514d3ee0.png)


