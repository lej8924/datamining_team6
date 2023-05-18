# DataMining Project- Team6

데이터마이닝 팀프로젝트 **6팀** 레포지토리입니다.
<table>
  <tr>
    <td align="center"><a href="http://ivesvh.com"><img src="https://avatars0.githubusercontent.com/u/587016?v=3" width="100px;" alt="양창록"/><br /><sub><b>양창록</b></sub></a><br /> <a href="https://github.com/codesandbox/codesandbox-client/commits?author=CompuIves" title="Documentation">📖</a></td>
    <td align="center"><a href="http://donavon.com"><img src="https://avatars0.githubusercontent.com/u/887639?v=3" width="100px;" alt="오성빈"/><br /><sub><b>오성빈</b></sub></a><br /><a href="https://github.com/codesandbox/codesandbox-client/commits?author=donavon" title="Code">💻</a></td>
    <td align="center"><a href="http://www.jeffallen.io/"><img src="https://avatars0.githubusercontent.com/u/5266810?v=3" width="100px;" alt="안수연"/><br /><sub><b>안수연</b></sub></a><br /><a href="https://github.com/codesandbox/codesandbox-client/commits?author=vueu" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/bengummer"><img src="https://avatars0.githubusercontent.com/u/1089897?v=3" width="100px;" alt="이은재"/><br /><sub><b>이은재</b></sub></a><br /><a href="https://github.com/codesandbox/codesandbox-client/commits?author=bengummer" title="Code">💻</a></td></table>

주제 : 생체 데이터로부터의 칼로리 소모량 예측

[데이콘 경진대회](https://dacon.io/competitions/official/236097/overview/description)의 데이터셋에 다양한 분석기법을 적용해보았습니다.

## 배경 및 필요성
자기관리에 대한 트렌드가 발전하는 시대속에서 건강과 운동의 중요성에 대한 인식 증가

칼로리 관리를 위해 건강 관련 애플리케이션에 대한 관심과 활용도 증가

칼로리 소모량을 추정하는 AI 모델 개발이 주목받고 있음 



## 분석목적

Requirements for the software and other tools to build, test and push 
- [Example 1](https://www.example.com)
- [Example 2](https://www.example.com)

## DataSet에 대한 소개

### 데이터 획득
데이터 획득 : AI 해커톤 플랫폼 ‘데이콘’의 ‘칼로리 소모량 예측’ 경진대회

### 데이터 이해
CSV 형식의 7500개의 Train, Test 데이터 

ID : 샘플 별 고유 id

Exercise_Duration : 운동 시간(분)

Body_Temperature(F) : 체온

BPM : 심박수

Height(Feet) : 키(피트)

Height(Remainder_Inches) : 키(피트 계산 후 더해야 할 키)

Weight(lb) : 몸무게(파운드)

Weight_Status : 체중 상태

Gender : 성별

Age : 나이

Calories_Burned : 칼로리 소모량(목표 예측값)

분석에 직접적으로 활용되지 않는 ‘ID’ 변수 제외

범주형 변수 : Weight_Status, Gender

수치형 변수 : 나머지

    Give the example
And repeat

    until finished

End with an example of getting some data out of the system or using it
for a little demo

### EDA

기술 통계량(평균, 표준편차, 분포 등) 계산, 데이터를 시각화하여 패턴이나 동향 파악

데이터의 변수 간 상관 관계 조사, 상관 행렬, 산점도 행렬, 히트맵 등을 사용하여 변수 간의 관계를 시각화하고 분석

### 전처리

회귀분석모형 수식을 간단하게 만들기 위해 상수항을 독립변수 데이터에 추가하는 상수항 결합작업 수행

target Y인 Calories_Burned'와 분석하는 데 필요없는 'ID열 제외

범주형 변수인 ‘Weight_Status’와 ‘Gender’ 정수형으로 변환

'Height'값을 inch로 단위 변환하면서 'Height(Remainder_Inches)'와 합침

'Height'와 ‘Weight’를 이용해 체질량지수 'BMI' 변수 추가

'Height', ‘Weight’, 'Age', 'Gender'를 이용해 기초대사율 'BMR' 변수 추가

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


