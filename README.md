# DataMining Project - Team6

데이터마이닝 팀프로젝트 **6팀** 레포지토리입니다.

<table>
  <tr>
    <td align="center"><a href="http://ivesvh.com"><img src="https://avatars0.githubusercontent.com/u/587016?v=3" width="100px;" alt="양창록"/><br /><sub><b>양창록</b></sub></a><br /> <a href="https://github.com/codesandbox/codesandbox-client/commits?author=CompuIves" title="Documentation">📖</a></td>
    <td align="center"><a href="http://donavon.com"><img src="https://avatars0.githubusercontent.com/u/887639?v=3" width="100px;" alt="오성빈"/><br /><sub><b>오성빈</b></sub></a><br /><a href="https://github.com/codesandbox/codesandbox-client/commits?author=donavon" title="Code">💻</a></td>
    <td align="center"><a href="http://www.jeffallen.io/"><img src="https://avatars0.githubusercontent.com/u/5266810?v=3" width="100px;" alt="안수연"/><br /><sub><b>안수연</b></sub></a><br /><a href="https://github.com/codesandbox/codesandbox-client/commits?author=vueu" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/bengummer"><img src="https://avatars0.githubusercontent.com/u/1089897?v=3" width="100px;" alt="이은재"/><br /><sub><b>이은재</b></sub></a><br /><a href="https://github.com/codesandbox/codesandbox-client/commits?author=bengummer" title="Code">💻</a></td></table>                

Repository 방문 횟수

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fgjbae1212%2Fhit-counter)](https://hits.seeyoufarm.com)    

## 01. 프로젝트 개요
### (1) 프로젝트 주제와 목적
#### "생체 데이터로부터의 칼로리 소모량 예측"

생체 데이터를 활용하여 개인의 칼로리 소모량을 예측하는 것 뿐만 아니라, 여러 전처리 과정과 분석 기법을 통해 다양한 관점의 결과물을 제시하고자 합니다.

### (2) 프로젝트의 배경 및 필요성
* 자기관리에 대한 트렌드가 발전하는 시대속에서 건강과 운동의 중요성에 대한 인식 증가

* 칼로리 관리를 위해 건강 관련 애플리케이션에 대한 관심과 활용도 증가

* 칼로리 소모량을 추정하는 **AI 모델** 개발이 주목받고 있음 


## 02. 분석
### (1) Dataset Info.

AI 해커톤 플랫폼 [데이콘](https://dacon.io/competitions/official/236097/overview/description)의 ‘칼로리 소모량 예측’ 경진대회 데이터셋을 활용하였습니다.

### (2) 전처리

### (3) 모델링

### (4) 후처리


범주형 변수 정수형으로 변환

체질량지수 'BMI', 기초대사율 'BMR' 변수 추가

히트맵으로 변수간의 상관관계 확인

[상관관계 높은 변수]

'BPM'-'Body_Temperature(F)', 'Exercise_Duration'

'Body_Temperature(F)'-'Exercise_Duration'

‘Weight’ - 'Height', 'BMI', 'BMR'

‘Weight_Status’ - 'BMI'

'Height' - 'BMR'

+ 요인분석 : 어떤 변수들간의 잠재요인(latent factor)에 있어 개별 변수들을 설명하고 있음을 통계적으로 도출하는 분석

상수항변수와 'BMI'를 포함하여 적합성 검정을 했더니 부적합하여 변수에서 제외

데이터가 요인분석에 적합한지 확인하기 위해 바틀렛 테스트와 KMO 검정 시행

바틀렛 테스트 결과 p-value가 0.05 이하, KMO 검정 결과 0.64로 0.5 이상이기 때문에 요인분석에 적합한 것으로 판단

최대우도 요인추출법을 사용하여 요인분석 시작

요인수 선택은 스크리 검정법을 통해 고유값이 1에 가장 가까운 요인 3개로 결정

요인은 세 개, 회전은 가장 많이 사용되는 varimax라는 직각회전을 사용하여 각 데이터값에 곱해질 고유벡터 dataframe 생성

이 데이터프레임에 대해 히트맵을 그려본 결과 원래의 데이터와 비슷한 상관관계를 가짐

세 개의 요인으로 축소된 요인점수 도출

+ T-SNE

x를 스케일링 후 각각 2차원, 3차원으로 시각화 -> 의미 찾아야 함

+ __다중공선성 해결__
  
  분산팽창요인이 매우 크게 나온 변수가 많아 다중공선성을 줄여야 할 것으로 판단
  
  + 다양한 변수 조합으로 VIF 해결
  
  변수들간의 상관관계가 높은 것들끼리 세그룹으로 나누고 VIF가 높은 변수들 중에서 서로 상관관계 있는 변수를 하나씩 제외하면서 VIF를 확인해본 결과, 상관관계가 높은 변수들은 없으나 다른 요인에 의해 다중공선성이 높게 나타나는 것으로 추측 
  
  + PCA
  
  PCA에 사용할 데이터 standard scaler로 스케일링
  
  component를 3개, 4개로 설정하여 각각 PCA 실행
  
  설명변수를 보았을 때 4개의 요인이 3개의 요인보다 더 높은 설명력을 가짐
  
  결과적으로 4개의 요인일 때 R-squared 값은 약 0.01 가량 높았고 다중공선성도 조금 더 높아졌음 
  
  + Polynomial
  
  앞선 PCA에서 나온 설명변수를 조금 더 높이고자 polynomial 결합 시행
  
  _Polynomial(degree = 2)+PCA(n=3)_
  
  3, 4개의 요인과 2, 3차원으로 다양한 조합을 시행했으나 3개의 요인으로 줄였던 PCA 값을 2차원으로 Polynomial 결합한 결과 R-squared값도 0.98정도로 높게 유지하면서, p-value가 0.05보다 높게 나온 feature의 갯수도 적어졌기 때문에 이 조합이 Best 조합으로 선정함
  
  + Filter Method 
  
  filter method는 변수를 미리 설정해놓고 Linear Regression을 실행시키는 방법으로 for문을 돌면서 좋은 결과를 낼 수 있는 변수를 도출 
  
  그 결과 선택된 네 개의 변수 : 'Age','Exercise_Duration','Gender','Weight_Status'
  

### 모델링

요인점수로 linear regression을 해본 결과 도출 + 해석 추가
Polynomial(degree = 2)+PCA(n=3) 이 조합으로 linear regression을 해본 결과 도출 + 해석 추가
filter method에서 선택된 네 개의 변수로 linear regression, Ridge를 해본 결과 도출 + 해석 추가

위 세 가지 경우 중 Polynomial(degree = 2)+PCA(n=3) 조합이 가장 높은 성능을 가짐

해당 데이터셋에 automl 적용 결과 도출 + 해석 추가

#### 1등코드에 대한 설명~~~~
```
Explain what these tests test and why

    Give an example
```
#### 2등코드에 대한 설명~~~~
```
Checks if the best practices and the right coding style has been used.

    Give an example
```
### 후처리

다양한 모델을 조합하여 예측 결과를 향상시키는 앙상블 기법 적용

1. stacking

2. voting

### 평가
> #### 알고리즘1
|  하이퍼파라미터1 | 하이퍼파라미터2 | accuracy  | 컬럼  | 컬럼  |
|---|---|---|---|---|
|  aa | bb  | cc  | dd  | ee  |
|  aa |bb   |  cc |  dd |  ee |
|  aa |  bb |  cc |  dd |  ee |

Checks if the best practices and the right coding style has been used.

> #### 알고리즘2
|  하이퍼파라미터1 | 하이퍼파라미터2 | accuracy  | 컬럼  | 컬럼  |
|---|---|---|---|---|
|  aa | bb  | cc  | dd  | ee  |
|  aa |bb   |  cc |  dd |  ee |
|  aa |  bb |  cc |  dd |  ee |

## 결과

Add additional notes to deploy this on a live system

### 해석

  - [Contributor Covenant](https://www.contributor-covenant.org/) - Used
    for the Code of Conduct
  - [Creative Commons](https://creativecommons.org/) - Used to choose
    the license

### 시사점

개인의 식습관과 대사율에 대한 통찰력 제공

음식 섭취에 대해 정보에 입각한 선택으로 더 건강한 생활 방식 영위

목표 달성을 위한 맞춤형 피트니스 루틴 개발 가능

### insights

We use [Semantic Versioning](http://semver.org/) for versioning. For the versions
available, see the [tags on this
repository](https://github.com/PurpleBooth/a-good-readme-template/tags).

## 한계점

  - **Billie Thompson** - *Provided README Template* -
    [PurpleBooth](https://github.com/PurpleBooth)

See also the list of
[contributors](https://github.com/PurpleBooth/a-good-readme-template/contributors)
who participated in this project.

## 추후 개선 방안

This project is licensed under the [CC0 1.0 Universal](LICENSE.md)
Creative Commons License - see the [LICENSE.md](LICENSE.md) file for
details


