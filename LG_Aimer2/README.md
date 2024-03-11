# 주제
##### MQL데이터를 활용하여 영업 기회 전환 고객을 선별하기 위한 AI모델 개발
![image](https://github.com/SSangJ/LG-Aimers/assets/137964478/51ea57ca-bbfc-4faa-96b8-0a9dfe0a419c)


<br>


# 파일 설명
## 1.train.csv [파일]
##### bant_submit :	MQL 구성 요소들 중 [1]Budget(예산), [2]Title(고객의 직책/직급), [3]Needs(요구사항), [4]Timeline(희망 납기일) 4가지 항목에 대해서 작성된 값의 비율
##### customer_country : 고객의 국적
##### business_unit :	MQL 요청 상품에 대응되는 사업부
##### com_reg_ver_win_rate : Vertical Level 1, business unit, region을 기준으로 oppty 비율을 계산
##### customer_idx : 고객의 회사명
##### customer_type : 고객 유형
##### enterprise : Global 기업인지, Small/Medium 규모의 기업인지
##### historical_existing_cnt : 이전에 Converted(영업 전환) 되었던 횟수
##### id_strategic_ver : (도메인 지식) 특정 사업부(Business Unit), 특정 사업 영역(Vertical Level1)에 대해 가중치를 부여
##### it_strategic_ver : (도메인 지식) 특정 사업부(Business Unit), 특정 사업 영역(Vertical Level1)에 대해 가중치를 부여
##### idit_strategic_ver : Id_strategic_ver이나 it_strategic_ver 값 중 하나라도 1의 값을 가지면 1 값으로 표현
##### customer_job : 고객의 직업군
##### lead_desc_length : 고객이 작성한 Lead Descriptoin 텍스트 총 길이
##### inquiry_type : 고객의 문의 유형
##### product_category : 요청 제품 카테고리
##### product_subcategory : 요청 제품 하위 카테고리
##### product_modelname : 요청 제품 모델명
##### customer_country.1 : 담당 자사 법인명 기반의 지역 정보(대륙)
##### customer_position : 고객의 회사 직책
##### response_corporate : 담당 자사 법인명
##### expected_timeline : 고객의 요청한 처리 일정
##### ver_cus : 특정 Vertical Level 1(사업영역) 이면서 Customer_type(고객 유형)이 소비자(End-user)인 경우에 대한 가중치
##### ver_pro : 특정 Vertical Level 1(사업영역) 이면서 특정 Product Category(제품 유형)인 경우에 대한 가중치
##### ver_win_rate_x : 전체 Lead 중에서 Vertical을 기준으로 Vertical 수 비율과 Vertical 별 Lead 수 대비 영업 전환 성공 비율 값을 곱한 값
##### ver_win_ratio_per_bu : 특정 Vertical Level1의 Business Unit 별 샘플 수 대비 영업 전환된 샘플 수의 비율을 계산
##### business_area : 고객의 사업 영역
##### business_subarea : 고객의 세부 사업 영역
##### lead_owner : 영업 담당자 이름
##### is_converted : 영업 성공 여부. True일 시 성공.



<br>

## 2.sample_submission.csv [파일] - 제출 양식

<br>




# 데이터 전처리 및 모델 설명

##### 이 문서는 데이터 전처리 및 모델 선택에 대한 접근 방식을 설명

## 1.데이터 전처리

##### 1-1) 데이터 정제: 사용하지 않는 customer_country.1, product_modelname 열을 제거합니다. 모든 문자열 타입의 데이터를 소문자로 변환하여 일관성을 유지

##### 1-2) 결측치 처리: 결측치를 다루기 위해 특정 열은 'none', '0', 'other'로 대체합니다. 이는 데이터의 특성에 따라 결정

##### 1-3) 데이터 형식 통일: customer_country 열에서 'none' 값은 '//none'으로 변경하여 형식을 통일

##### 1-4) 데이터 빈도 처리: 빈도수가 낮은 데이터를 '-1' 또는 'other'로 치환하고, 특정 값들을 통합하여 데이터의 일관성을 높임

##### 1-5) 국가명 표준화: country 열을 생성하고, 다양한 표현을 통일된 국가명으로 매핑

##### 1-6) 파생변수 생성: lead_owner의 변환율을 기반으로 새로운 열을 추가하고, 특정 조건에 따라 새로운 파생변수를 생성

##### 1-7) 스케일링 : 수치형 변수에 대해 표준화를 적용하여 변수 간 스케일 차이를 조정
<br>

## 2. 결측치 대체

##### 결측치를 다루기 위해 특정 열은 'none', '0', 'other'로 대체합니다. 이는 데이터 feature를 분석한 후 결정

<br>

## 3. 타겟 인코딩
##### 3-1) 카테고리형 변수가 대부분 이였기 때문에 label 인코딩/ target 인코딩/ one-hot 인코딩 중에서 target 인코딩 결정
##### 3-2) target 인코딩을 설정하면 overfitting의 가능성이 높기 때문에 feature를 분석하여 빈도수가 적은 고유값에 대해서는 평균값으로 대체하여 overfitting 방지

<br>

## 4. 데이터 불균형
##### 4-1) 고객의 전환율을 나타내기 때문에 전환하는 고객의 비율은 약 0.1로 데이터의 불균형이 존재하였음
##### 4-2) SMOTE와 Tomeklinks 조합을 사용하여 클래스 불균형 문제를 해결



## 3. 예측 모델

### 3.1 MLP 모델(사용 x)
##### 간단한 MLP 모델을 사용하여 다양한 규제 방법을 이용하여 layer수, layer 깊이 등을예측 해 보았으나 매우 낮은 validation f1 score를 보여 사용하지 않음


### 3.2 머신 러닝 모델
##### XGBoost, LightGBM, CatBoost ,DecisionTruee, ensenble, 다양한 알고리즘을 실험합니다. Optuna를 사용한 하이퍼파라미터 최적화를 통해 최적의 모델 설정을 찾음
##### 최종 모델은 XGBoost 모델을 이용하였음
