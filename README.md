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

  

AI 해커톤 플랫폼 [데이콘](https://dacon.io/competitions/official/236097/overview/description)의 ‘칼로리 소모량 예측’ 경진대회 데이터셋을 활용하였습니다.

  

### (2) 전처리

  

* 범주형 변수 -> 정수형 변수 변환

* 다중공선성 문제 해결

  * 차원 축소
  
    * VIF - Filter Method

    * 요인분석

    * PCA

* PolynomialFeatures : 부족한 설명 변수의 개수를 늘리기 위해

  

### (3) 모델링

  

* LinearRegression

* Ridge : overfitting을 방지하기 위해

* 교차검증

  

### (4) 후처리

  

* 반올림 : label값이 정수이므로 예측의 정확도를 높이기 

* 모델 앙상블

* Stacking


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
|Stacking|Linear Regression|0.976|95.89|
|Stacking|Ridge Regression|0.976|95.89|

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
