# DataMining Project - Team6

  

:bar_chart: 데이터마이닝 팀프로젝트 **6팀** 레포지토리입니다.

  

<table>

<tr>

<td  align="center"><img  src="https://github.com/lej8924/datamining_team6/assets/131632489/f3eda3f7-f227-4bb4-96c4-09fb6b6f7e1a"  width="120px;"  alt="양창록"/><br /><b>양창록</b><br />산업정보시스템전공<br />20110198</td>

<td  align="center"><img  src="https://github.com/lej8924/datamining_team6/assets/131632489/d8463a63-8eca-4558-a2c8-232bfe6ae773"  width="120px;"  alt="오성빈"/><br /><b>오성빈</b><br />산업정보시스템전공<br />19102003</td>

<td  align="center"><img  src="https://github.com/lej8924/datamining_team6/assets/131632489/4d8fd099-4e23-4414-b26d-1377eb01db10"  width="120px;"  alt="안수연"/><br /><b>안수연</b><br />산업정보시스템전공<br />20102031</td>

<td  align="center"><img  src="https://github.com/lej8924/datamining_team6/assets/131632489/f51c9fe4-a0d9-4b99-a3e2-45c830d003c9"  width="120px;"  alt="이은재"/><br /><b>이은재</b><br />산업정보시스템전공<br />20102040</td></table>

  

>레포지토리 방문 횟수<br>

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Flej8924%2Fdatamining_team6%2Fblob%2Fmain%2FREADME.md&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

  

<br>

  

## 01. 프로젝트 개요

  

###  프로젝트 주제와 목적

  

> "생체 데이터로부터의 칼로리 소모량 예측"

  

생체 데이터를 활용하여 개인의 칼로리 소모량을 예측하는 것 뿐만 아니라, 여러 전처리 과정과 분석 기법을 통해 다양한 관점의 결과물을 제시하고자 합니다.

  

### 프로젝트의 배경 및 필요성

<img src="https://dimg.donga.com/wps/NEWS/IMAGE/2021/04/12/106357558.1.jpg" width="70%">

* 자기관리에 대한 트렌드가 발전하는 시대속에서 :muscle:건강과 :running:운동의 중요성에 대한 인식 증가

* 칼로리 관리를 위해 건강 관련 애플리케이션에 대한 관심과 활용도 증가

* 칼로리 소모량을 추정하는 **AI 모델** 개발이 주목받고 있음

  

<br>

  

## :mag:02. 분석

  

### (1) 데이터셋 소개

  
<img src="https://dacon-images.s3.ap-northeast-2.amazonaws.com/dacon20220919.png" width="50%">

AI 해커톤 플랫폼 [데이콘](https://dacon.io/competitions/official/236097/overview/description)의 *칼로리 소모량 예측* 경진대회 데이터셋을 활용하였습니다.

  

### (2) EDA
<table style="border : none;">
  <tr>
  <td><img src="https://github.com/lej8924/datamining_team6/assets/71022086/c6d96afc-30ef-4cb7-93d1-3b841172ca63?"></td>
    <td><img src="https://github.com/lej8924/datamining_team6/assets/71022086/7250ce88-548d-4c79-8a30-f9a43d850a39"></td>
  </tr>
</table>

#### 데이터 가공

* 범주형 변수 -> 정수형 변수 변환
```python
#범주형 변수 처리해주기
x["Weight_Status"] = x["Weight_Status"].replace(['Normal Weight','Overweight','Obese'],[1,2,3])
x["Gender"] = x["Gender"].replace(['M','F'],[1,2])
```
* 불필요한 column 통합 및 삭제
```python
#Height값을 inch로 통합하는데, 소수점 나머지 inch값도 같이 포함시켜줌
x['Height(inch)'] = x['Height(Feet)'] * 12 + x['Height(Remainder_Inches)']
# Height는 feet->inch로 변경하였고, remainder_inches는 이제 불필요한 값이므로 데이터프레임에서 삭제해준다
x = x.drop(['Height(Feet)', 'Height(Remainder_Inches)'], axis=1)
```
* 도움이 될 BMI,BMR 컬럼에 추가
<img src="https://www.diettechafrica.com/wp-content/uploads/2020/05/Body-mass-index.png" width="40%">

```python
# 키와 체중으로 비만율인 BMI변수 추가해보기
x['BMI'] = (x['Weight(lb)'] / x['Height(inch)'] ** 2) * 703
# BMR은 기초대사량인데, 운동과 관련이 있을까 해서 추가함
x['BMR'] = 10 * x['Weight(lb)'] * 0.453592 + 6.25 * x['Height(inch)'] * 2.54 - 5 * x['Age'] + x['Gender'].apply(lambda x: 5 if x=='M' else -161)
```

* 데이터 상관관계 분석
<img src="https://user-images.githubusercontent.com/71022086/239699439-05e797a3-040a-49c6-8019-3e7d9b4feae5.png" width="50%">

* TSNE
 - 3차원으로 나타냈을 때 크게 두 개의 군집을 형성하는 것이 보여진다.
 - 우리의 주관적인 해석으로는 *남자*와 *여자*로 각각 나누어 생각하였다.
 - ![한국의료재단](https://hworld.org/281)에 따르면, 호흡, 호르몬, 근골격, 지구력 등에서 성별간 운동능력 차이가 존재한다고 하였기 때문이다.
<img src="https://user-images.githubusercontent.com/71022086/239699450-a75f5c95-00a1-4a86-a4fe-29541985d001.png">


#### 다중공선성 문제 해결
- 데이터를 따로 처리해주기 전에는 sm.OLS의 **Cond.No. = 1.30e+17**로 매우 높게 나옴
- 따라서 차원축소 혹은 filter method를 사용하여 다중공선성이 높은 컬럼간의 결합 혹은 몇 가지 컬럼을 선택하기로 

* 차원 축소

  (1) 요인분석
    - 요인 수 선택: 스크리도표를 통해 *n_factors = 3* 이 최적의 hyperparameter임을 알게 됨.
    - rotation = "varimax" ,method = "ml"
    - fa_score 계산 후 sm.OLS분석을 실시해봄
    ```python
    fa = FactorAnalyzer(n_factors=3,method ='ml', rotation="varimax")
    ```

  (2) PCA
     - hyperparameter인 n_components를 3,4 각각으로 나누어 실시해봄
     - 앞선 요인분석에서도 3이 나왔기에 그것을 근거로 함
     ```python
     pca_3 = PCA(n_components=3,random_state=3)
     pca_4 = PCA(n_components=4,random_state=3)
     ```

  

  - 각각의 전처리 경우를 ols를 통해서 성능 비교, 최고 성능 전처리 선택 
* Filter Method
  - vif함수를 통하여 column을 선택함
  - 선택된 columns : **Age,Exercise_Duration,Gender,Weight_Status**
    ```
    1번째 VIF 측정
    Max VIF feature & value : BMR, 4015891.9971667505
    2번째 VIF 측정
    Max VIF feature & value : Body_Temperature(F), 12089.836089534592
    3번째 VIF 측정
    Max VIF feature & value : Weight_Status, 959.8108350380234
    4번째 VIF 측정
    Max VIF feature & value : Weight_Status, 324.28407125332956
    5번째 VIF 측정
    Max VIF feature & value : BPM, 138.26497227573296
    6번째 VIF 측정
    Max VIF feature & value : Exercise_Duration, 18.89230596188942
    7번째 VIF 측정
    Max VIF feature & value : Age, 7.260281236519285


    Age의 vif는 7.26입니다.
    Exercise_Duration의 vif는 4.08입니다.
    Gender의 vif는 5.32입니다.
    Weight_Status의 vif는 5.55입니다.
    ```
#### R<sup>2</sup>score 높이기
* PolynomialFeatures

```python
poly3 = PolynomialFeatures(degree = 3)
poly2 = PolynomialFeatures(degree = 2)
```

* 반올림 : label값이 정수이므로 예측의 정확도를 높이기 위함.
```python
Pred1 = np.round(Pred1)
Pred2 = np.round(Pred2)
```


### (3) 모델링

* pipeline
  - 앞선 단계 중 가장 좋은 성능을 낸 pca(n=3),polynomial(d=2)를 pipeline으로 함축적으로 나타낸다.
  ```python
  pipeline_LR = Pipeline([
    ('scaler',StandardScaler()),
    ('pca',PCA(n_components=3,random_state=3)),
    ('poly', PolynomialFeatures(degree=2)),
    ('linear', LinearRegression())])
    
  pipeline_Ridge = Pipeline([
      ('scaler',StandardScaler()),
      ('pca',PCA(n_components=3,random_state=3)),
      ('poly', PolynomialFeatures(degree=2)),
      ('ridge', Ridge())])
    ```

  

* 앙상블 - stacking

  * 새로운 설명 변수를 만들기 위한 모델 : LinearRegression, Ridge

  * Automl 모듈의 TabularPredictor() - 여러 모델들을 비교해서 가장 성능이 좋은 모델을 즘찾아주는 알고리즘
  ```python
  stacking = TabularPredictor(label='Calories_Burned', eval_metric='rmse', problem_type='regression').fit(new_train, presets=['best_quality'], num_stack_levels=0)
  ### rmse측면에서 가장 좋은 성능을 낸 모델인 'WeightedEnsemble_L2' 사용
  Pred = stacking.predict(NEW, model = 'WeightedEnsemble_L2')
  ```


* *cross val_score*를 활용한 교차검증 
```python
scores = np.sqrt(-cross_val_score(pipeline_LR, x,y, cv=5, scoring=custom_scoring_function))
#Cross-validation RMSE of linear regression: 9.61123741317941

scores = np.sqrt(-cross_val_score(pipeline_Ridge, x,y, cv=5, scoring=custom_scoring_function))
#Cross-validation RMSE of Ridge regression: 9.611236144803346

scores = np.sqrt(-cross_val_score(pipeline_LR, x_test,y_test, cv=5, scoring=custom_scoring_function))
#Cross-validation RMSE of linear regression: 9.859088058138038

scores = np.sqrt(-cross_val_score(pipeline_Ridge, x_test,y_test, cv=5, scoring=custom_scoring_function))
#Cross-validation RMSE of Ridge regression: 9.86044649442146

```


## :bulb:test 결과

|분석방법|HyperParameter|R<sup>2</sup>|# of(P-value>0.05)|Cond.No.|
|---|---|---|---|---|
|Filter Method| - |0.931|0|1.91|
|요인분석|n_factors=3|0.915|0|1.06|
|PCA|n_component=3|0.906|0|2.10|
|PCA|n_component=4|0.911|0|2.50|
|PCA + polynomial|**<span style="background-color:#fff5b1"> n_component=3,degree = 2 </span>**|0.977|0|13.9|
|PCA + polynomial|n_component=3,degree = 3|0.979|0|46.9|
|PCA + polynomial|n_component=4,degree = 2|0.984|0|17.4|
|PCA + polynomial|n_component=4,degree = 3|0.911|0|2.50|
<br>

|모델 앙상블|분석모델|R<sup>2</sup>|MSE|
|---|---|---|---|
|X|Linear Regression|0.976|96.33|
|X|Ridge Regression|0.976|96.32|
|**Stacking**|Linear Regression|0.976|95.89|
|**Stacking**|Ridge Regression|0.976|95.89|

## 03. 시작 가이드

  



  

*  ![](https://img.shields.io/badge/python-3.7-blue?logo=python)

* ![](https://img.shields.io/badge/jupyter-notebook-orange?logo=jupyter)

### requirements
```
seaborn
statsmedels
factor_analyzer
autogluon
```
  

### Installation
* Clone the repo

```bash

git clone https://github.com/lej8924/datamining_team6.git

cd datamining_team6

```

  
* Run the project 
```bash

python code.py

```

<br>

  

## 04. Stacks

  

### Environment

<img  src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white"> <img  src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=gitHub&logoColor=white">

  

### Development

<img  src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white">

  

### Communication

![GoogleMeet](https://img.shields.io/badge/GoogleMeet-00897B?style=for-the-badge&logo=Google%20Meet&logoColor=white)

  

<br>

  

## 05. 라이센스

  

MIT 라이센스를 따릅니다. 자세한 내용은 `License` 파일을 확인해주세요.

  

<br>

  

## 06. 프로젝트에 기여하기

  

이 프로젝트는 오픈소스입니다. 당신의 기여를 통해 이 프로젝트를 발전시켜 주세요.

  

[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](code_of_conduct.md)
