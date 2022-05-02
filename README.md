# photo synthesis
development for sim 2 real 


## 기업 과업 소개
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/70171637/166200515-ed453473-008d-4ea7-9bbc-e37e7e92dd51.png">
- 실제데이터는 거리별 Blur 효과처럼 흐릿해지지만 합성데이터 환경에서는 거리별 차이없이 선명하게 나타남
- SIM2REAL팀은 가상환경 내 거리별 차이를 구현중에 있으며 이번 과제에서는 별도 OpenCV와 같은 영상처리 기법으로 데이터 증강 또는 같이 주어지는 Depthmap or Segmentation 이미지 정보를 통해 새로운 아이디어 제시 가능

--------------------------
## 원인 분석

현실세계에서 멀리있는 물체가 흐릿해져 보이는 이유
- 먼지로 부터의 빛의 산란으로 인한 이미지 블러
- 렌즈의 초점거리로 부터 벗어난 부분의 이미지 블러
- 카메라 frame과 물체의 속도 차이로 인한 블러
- 이 밖에 비나 다른 자연환경에 의해 얻을 수 있는 블러는 너무 다양하다.

=>  일단 날씨가 밝을때 찍고 카메라 파라메타를 아는 kitti datset에서의 거리별 blur의 특징을 구해보자

----------------------------------------
## 거리별 blur 를 확인하는 방법

1. depth를 통한 이미지 분류
- kitti dataset에 있는 detph map을 통하여 값의 구간을 나누어 분류한다.
	- 구간을 나누는 기준을 생각

2. 거리별로 분류한 이미지의 blur metric을 구하기
blur metric이란?[1]
> NRQA(No-reference quality assessment) 평가 방법 중 하나로 블러(blur)의 양을 측정하는 객관적 화질평가인 블러 메 트릭(blur metric)은 에지의 시작과 끝 사이의 폭을 측정함 으로써 블러의 정도를 추정하는 방법이다. 먼저 생성된이미지 에지를 검출한 후에, 각 에지 픽셀에서 국부 최대 (local maximum)를 갖는 픽셀의 위치 ps , 국부 최소(local minimum)를 갖는 픽셀의 위치 pf를 이용하여 에지의 폭 wk=ll ps-pfll를 계산한다. 에지는 1 픽셀 이상의 폭을 가진 다. 모든 스캔라인에서 wk를 계산한 후에, 블러 메트릭 BM 은 아래식과 같이 wk의 합으로 구한다

2. 1 depth를 통한 분류별 이미지를 edge detection을 하여 blur meric을 구한다.

$BM = \displaystyle\sum_{i=1}^{M}\displaystyle\sum_{i=1}^{K} W_k,i/ \displaystyle\sum_{i=1}^{M}K_i$

2. 2 

3. depth별로 계산된 blur metric을 선형회귀 모델에 넣기

depth를 x로 blur metric값을 y로 하여 데이터를 넣어 depth별 blur 예측모델을 만든다.

4. 합성데이터로 depth estimation을 하거나 stereo detph를 구하여 depth별 blur를 적용시켜 domain gap을 줄여본다.


---------------------------------------------------------------------------------
예상 되는 문제점과 더 생각해볼점
- 어떠한 블러를 적용시킬지
	- 일단 gaussian blur를 생각하고 있다.
- depth map을 통해 이미지를 분리할때 어떻게 이미지를 자르고 padding을 해야할지
	- 전체 이미지를 edge detection을 한 이후에 분리하는 것이 나을 수도 있단 생각이든다.
- 어떤 edge detection을 사용할 것인지
	- canny를 생각하고있긴하다.
- Blur metric을 구현할 방법 생각 
	- scikit image에서 제공되는 모듈 사용 [2]
	- 혹은 직접구현
- 어떠한 선형회귀 모델을 사용 할 것인지
	- 혹은 다른 모데을 사용할 것인지

- 합성데이터의 자동차에만 blur 정도를 적용시켜 DR 효과를 줄 수 있을까?

-------------------------------------------------------------
# refference 

[1] Analysis of Relationship between Objective Performance Measurement
and 3D Visual Discomfort in Depth Map Upsampling
Jong In Gila), Saeed Mahmoudpoura), and Manbae Kima)‡  
[2] 



## 요약


## 서론(이전 선행되었던 결과 및 문제점, 해결방안)
기업측 참고 논문에서 moraiSIM과 domain randomization을 통해 도메인 갭을 줄이는 방법을 제시
하지만 악천후와 같은 상황, 작은 bounding box에서의 상황에서 성능이 제대로 안나오는 문제점 발생
domain randomizaion과 같은 다양한 시도들이 진행되는 중입니다.

보통 자율주행 연구에서 많이 사용하는 kitti와 vkitti 데이터셋을 논문에서 많이 사용
저희도 kitti와 vkitti를 기본적으로 사용, 국내 도메인의 합성데이터로 검증?



## *base line code*


## 본론 (데이터셋, 모델학습 등의 내용)

 
## 결과 및 분석(검증)


## reference
기업측 참고 논문: 자율주행 차량의 학습 데이터 자동 생성 시스템 개발,
              Training Deep Networks with Synthetic Data: Bridging the Reality Gap by Domain Randomization
kitti dataset : http://www.cvlibs.net/datasets/kitti/eval_scene_flow.php?benchmark=stereo
vkitti dataset : https://arxiv.org/pdf/1605.06457.pdf

블러 매트릭스 관련 논문
