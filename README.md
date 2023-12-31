# 파일 설명

## 1.train.csv [파일]
##### ID : 실제 판매되고 있는 고유 ID
##### 제품 : 제품 코드
##### 대분류 : 제품의 대분류 코드
##### 중분류 : 제품의 중분류 코드
##### 소분류 : 제품의 소분류 코드
##### 브랜드 : 제품의 브랜드 코드
##### 2022-01-01 ~ 2023-04-04 : 실제 일별 판매량

<br>

## 2.sample_submission.csv [파일] - 제출 양식
##### ID : 실제 판매되고 있는 고유 ID
##### 2023-04-05 ~ 2023-04-25 : 예측한 일별 판매량

<br>

## 3.sales.csv [파일] - 메타(Meta) 정보
##### ID : 실제 판매되고 있는 고유 ID
##### 제품 : 제품 코드
##### 대분류 : 제품의 대분류 코드
##### 중분류 : 제품의 중분류 코드
##### 소분류 : 제품의 소분류 코드
##### 브랜드 : 제품의 브랜드 코드
##### 2022-01-01 ~ 2023-04-04 : 실제 일별 총 판매금액

<br>

## 4.brand_keyword_cnt.csv [파일] - 메타(Meta) 정보
##### 브랜드 : 브랜드 코드
##### 2022-01-01 ~ 2023-04-04 : 브랜드의 연관키워드 언급량을 정규화한 일별 데이터

<br>
<br>

# 데이터 전처리 및 모델 설명

이 문서는 데이터 전처리 및 모델 선택에 대한 접근 방식을 설명합니다.

## 1. 이상치 처리

### 1.1 IQR 방식

처음에는 IQR 방식을 사용하여 이상치를 처리하려 했지만, 이로 인해 결측치로 처리되는 값이 너무 많아 성능이 떨어졌습니다.

### 1.2 수정된 이상치 처리

따라서, 25~75퍼센타일 범위가 아닌 대신 1퍼센타일부터 99퍼센타일까지 *2.5배를 넘어가는 값을 제거하기로 결정하였습니다.

<br>

## 2. 결측치 대체

이상치가 제거된 값은 지수평활법을 이용하여 대체하였습니다.

<br>

## 3. 예측 모델

### 3.1 선형 회귀 모델

브랜드 키워드 언급량을 이용하여 간단한 선형 회귀 모델을 구축하였습니다. 또한, 최근 검색량 동향도 모델에 포함시켰습니다.

### 3.2 순환 신경망 (GRU)

LSTM, GRU, BILSTM 등 다양한 모델을 고려했으며, GRU 방식을 채택하여 모델을 구축하였습니다.

이로써 데이터 전처리와 모델 설명이 완료되었습니다.
