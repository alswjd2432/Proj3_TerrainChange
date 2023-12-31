# 항공위성 이미지 학습을 통한 지형 변화 탐지 모델
## 목표 : 항공 혹은 위성 이미지를 입력 시, 지형의 구조물(건물, 주차장, 도로, ...)을 segmentation 해주는 모델을 만들고, 나아가 지형 변화를 탐지해주는 모델 구축

### 데이터 :
- 제주도 항공위성 데이터 : https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=643
- 인조 홍수 항공 데이터 : https://www.kaggle.com/datasets/samiwood/synthetic-flood-imagery-for-image-segmentation
- 자체 제작 건물 붕괴 데이터 : /지형변화+건물붕괴/건물붕괴_512 폴더에 존재

<제주도 항공위성 데이터 예시><br>
![image](https://github.com/alswjd2432/Proj3_TerrainChange/assets/95081711/348d9da6-a3b4-4047-875a-123879482c84)
![image](https://github.com/alswjd2432/Proj3_TerrainChange/assets/95081711/4266745a-bbd3-4302-8b63-1efa1169e519)
<br>
<인조 홍수 항공 데이터 예시><br>
![image](https://github.com/alswjd2432/Proj3_TerrainChange/assets/95081711/863f205c-79cd-4973-9016-64752534dc41)
<br>
<자체 제작 건물 붕괴 데이터 예시 (라벨데이터 없음)><br>
![image](https://github.com/alswjd2432/Proj3_TerrainChange/assets/95081711/1dd6ac16-f100-415d-bada-04e382b6cf3c)


### 사용한 모델
1. UNet
2. UNet++
3. DeepLabV3
4. PSPNet <br>
(github 코드에는 unet을 사용한 코드만 있지만, segmentation_models_pytorch의 내장모델을 이용했기 때문에
model 부분의 unet을 변경하면 원하는 모델로 학습할 수 있다.)

### 학습 방법
데이터를 넣고 최적의 결과를 찾아주는 파라미터 및 인코더를 찾는다.
성능 최적화를 위해 조정한 것 : 모델 종류, 인코더 종류, 에폭, 학습률, 활성화 함수 등

#### 학습 1.
제주도 위성 데이터를 UNet에 train, validation, test 하면서 지형의 구조물을 segmentation 해주는 지 확인한다.
<br>
지진이나 자연재해로 건물이 붕괴되었을 상황을 가정하고, 지형의 원본, 1차 붕괴, 2차 붕괴 이미지를 학습한 모델이 예측하게 한다.
<br>
붕괴가 심할 수록 원본 모델 예측 이미지와 지형 붕괴 이미지 간의 이미지 유사도가 떨어지는 것을 확인한다.

#### 학습 2.
인조 홍수 데이터를 UNet에 train, validation, test 하면서 지형의 구조물과 홍수를 segmentation 해주는 지 확인한다.

### 결과
- 사용한 모델 중에서는 resnet101 인코더에 UNet 모델을 사용했을 때, 가장 성능이 좋았다.
- 사람이 라벨링한 원본 라벨링 데이터에서는 표시하지 않았던 구조물까지 모델이 예측해주는 것을 볼 수 있었다.
- 지형이 변할 수록 이미지 유사도가 떨어지는 것을 보아, 이상 지형 변화 프로그램을 구축할 때 활용할 여지가 있다.
- 홍수 이미지의 경우, 인조 홍수 이미지가 퀄리티가 좋지 않아서 그런지 홍수 외의 다른 구조물들은 잘 예측하지 못하는 결과가 나왔다.
<br>
<제주 위성 이미지 예측 결과><br>
<img width="543" alt="image" src="https://github.com/alswjd2432/Proj3_TerrainChange/assets/95081711/73a5d910-0ac8-4b6a-91c8-1f1e66e42b00">
<br>
<붕괴 이미지 예측 결과(앞 데이터 예시 참고)><br>
<img width="1021" alt="image" src="https://github.com/alswjd2432/Proj3_TerrainChange/assets/95081711/2e8efbeb-27a1-4748-9741-3d287dea909b">
<br>
<홍수 이미지 예측 결과><br>
<img width="780" alt="image" src="https://github.com/alswjd2432/Proj3_TerrainChange/assets/95081711/dc6303e2-e595-43e7-99ab-87d9253490cc">

##### 자세한 사항은 '항공위성 이미지 데이터로 지형 변화 파악하기.pdf' 확인









