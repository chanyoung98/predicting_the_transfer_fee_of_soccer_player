# predicting_the_transfer_fee_of_soccer_player

데이터셋 출처 : https://www.kaggle.com/datasets/kriegsmaschine/soccer-players-values-and-their-statistics

## 1. 개요
- 빅데이터 분석의 발전은 스포츠에서 다양한 분야에 활용되며, 축구산업에서도 전술 훈련, 개인 훈련 등 다양한 방면으로 활용되고 있다.
- 비즈니스적인 측면에서 축구선수의 역량에 따른 이적료 측정은 항상 중요한 화제거리였기에 이 또한 선수들의 데이터를 바탕으로 이적료에 영향을 주는 변수는 무엇인지 파악하고, 이적료를 예측하는 모델을 개발하고자 하였다.
- 17-18시즌 ~ 19-20시즌 3년 간의 유럽 5대리그 축구 선수 데이터를 사용하여 분석하였다.

## 2. 데이터 전처리
- 결측치 처리 및 중복 확인
  - 3개 데이터를 결합 후 중복된 데이터를 삭제
  - 나이(age), 출생년도(birth_year)등과 같이 동일한 정보를 내포하는 변수 제거
- 인코딩
  - LABEL 인코딩
- 데이터 분할
  - 골키퍼 관련 변수(_gk)를 활용하여 필드플레이어(fp)와 골키퍼(gk)로 데이터를 분할
  - 각 데이터를 활용하여 fp 분석과 gk 분석을 따로 진행
- 변수선택
  - 200개 이상의 변수를 모델링할 시 과적합의 위험이 있기에 단계적 선택법을 통해 의미있는 변수 선택
  - VIF가 10이상인 변수를 제거하여 다중공선성 방지
  - PCA(주성분분석)를 통해 분산을 유지시킴과 동시에 특정 변수들을 차원축소
- 정규화
  - 특정 변수들의 경우 좌측으로 치우친 분포를 따르기에 보다 효과적인 회귀분석을 위해 로그변환을 통해 정규화 진행

## 3. 데이터 모델링
- XGBoostRegression, RandomForestRegression, BaysianRidge, Ridge, Lasso, LinearRegression와 같은 회귀 모델을 사용하여 모델링 과정을 진행
- fp, gk 둘 다 성능이 가장 좋았던 XGBoostRegression을 최종 모델로 선정
- Parameter Tuning : Optuna
- XGBoost 내부적으로 결정트리의 불순도를 기반으로 feature importance 계산 -> 유의미한 변수 고찰

## 4. 결과 해석
- fp, gk 각각의 모델의 RMSE 값이 3.88772, 2.51743로 상당히 낮은 값을 도출
- feature importance를 계산한 결과
  - 필드플레이어 기준으로 CL(챔피언스리그 출전 여부), age(나이), league(소속 리그)등의 변수가 이적료 측정에 중요한 영향을 미치는 변수임
  - 골키퍼 기준으로 passes_launched_gk(시작 패스 횟수), clean_shees(클린 시트), age(나이)등의 변수가 이적료 측정에 중요한 영향을 미치는 변수임

  
