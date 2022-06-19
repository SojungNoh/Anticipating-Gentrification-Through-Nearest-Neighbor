# Anticipating-Gentrification-Through-Nearest-Neighbor
이 프로젝트는 석사 첫 학기 카이스트 건설환경공학과 도시연구특론(CE481(C)) 수업에서 간단하게 진행해본 파일럿 프로젝트다. 이 수업은 건설환경공학과 내의 Urbanism 수업으로, 'City Reader'라는 책으로 도시역사의 milestone이 된 연구들을 소개하고, 원문을 읽는 인문학 수업이다. 수업은 최종적으로 Term Project로 개인 연구와 관련된 파일럿 프로젝트를 각자 진행하게 되는데, Urbanism 안에서도 상권과 상업용 부동산의 변화 방향을 예측하는 것이 내 연구 주제다보니, Term Project도 '그 다음에 뜰 상권'(젠트리피케이션)을 예측하는 프로젝트를 진행했다.    


## Needs & Goals
젠트리케이션은 학계/언론에서 기존 주민을 몰아내는 부정적인 도시현상으로 비춰지곤한다. 
하지만 젠트리피케이션은 지역가치가 상승함에 따라 주요 거주민이 뒤바뀌는 현상으로 긍정적/부정적 측면이 공존하는 현상이다. 
젠트리피케이션은 낙후된 지역 가치를 활성화한다; 외부 자본의 유입, 소비 증가, 고용 증가, 부동산 가격 상승 등이 이 현상으로부터 파생되는 긍정적 부분들이다.
이를 통해 이익을 취하고자 하는 집단은 다양하지만, 크게 세 유형이 있을 수 있다고 판단했다; '
- 잠재성을 가진 토지를 발굴하고 싶어하는 부동산 투자자
- 블루오션을 찾고 있는 예비 창업자
- 관광을 통해 지역 활성화를 도모하고자 하는 지방자치단체
따라서 이 프로젝트는 젠트리피케이션을 *비즈니스 기회*로 바라보며, 잠재적인 투자처를 제공/추천하는 것을 목표로 한다.
예측은 *추천시스템*의 Collaborative Filtering 프레임워크의 유사도 측정을 기반으로 하며, 향후 5년 이내 젠트리피케이션이 발생할 상권을 예측하는 것이 프로젝트의 목표다.

##### Research Question
투자자의 관점에서, 잠정적인 상권을 예측하기 위한 방법으로 추천시스템의 유사도분석 프레임워크가 과연 적절한 방법론인가?


## Contents
1. Boudary Setting
2. Data Processing
3. Similarity Analysis
4. Verification
5. Conclusion

![캡처](https://user-images.githubusercontent.com/52244004/174471962-6daa97b6-3f7f-4e94-b260-ab24945a4e98.PNG)

### Packages
- [sklearn.preprocessing.MinMaxScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)
- [sklearn.metrics.pairwise.cosine_similarity](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html)

### 1. Boundary Setting
예측의 범위는 서울시이며, 그 단위는 '길'이다. [우리마을가게상권분석서비스](https://golmok.seoul.go.kr/main.do)에서 제공하는 서울시 골목상권 데이터셋을 사용 이 데이터는 서울형 골목상권을 분석하기 위해 서울시에서 직접 수집한 데이터셋이다. 대로변이 아닌 거주지 안의 좁은 도로를 따라 형성되는 상업세력의 범위를 의미하며, '길'기준 블록 단위 데이터다.

![캡처1](https://user-images.githubusercontent.com/52244004/174471972-078eeddf-8c4f-42b5-b618-d1a10a6531c2.PNG)
![캡처2](https://user-images.githubusercontent.com/52244004/174471977-257dcaa4-0c68-4bf1-9a0b-9944a525e0f0.PNG)

### 2. Data Processing
제공하고 있는 데이터는 총 10개로; 생활인구 / 상주인구 / 직장인구 / 점포 / 집객시설 / 아파트 / 추정매출 / 소득소비 / 상권변화지표 / 상권영역이다.   
(1) 유사도 분석을 진행하려면 Street Code x Feature Matrix가 필요한데, 이 10개 항목을 Street Code 별로 분류할 수 있는 데이터셋은 상주인구 / 직장인구 / 집객시설 / 추정매출 / 상권변화지표 5개 요소이기에, 이 다섯개를 활용해 Matrix(1209 골목상권 x 9 Feature)를 만들었다.   
(2) 또한 데이터는 분기단위로 제공되는데, 효율적인 작업 진행을 위해 년 단위로 환산하는 작업을 해 주었다.  
(3) 마지막으로 유사도 분석을 하는 과정에서, 특정 Feature의 값이 지나치게 크면, 그 Feature에 대한 bias가 발생할 수 있기 때문에 정규화하는 과정을 거쳤다.   

![캡처3](https://user-images.githubusercontent.com/52244004/174471982-706dab25-3e6e-4797-ba19-8190f782dbf3.PNG)
![캡처4](https://user-images.githubusercontent.com/52244004/174471984-335b8fb7-b5e6-450d-a343-82a7409f85cc.PNG)

### 3. Similarity Analysis
본 연구에서 이야기하는 유사도 분석은, 추천시스템의 한 종류인 Collaborative Filtering의 한 방법이다. 한 유저 A의 특정 아이템에 대한 선호도를 추정하는데 쓰이는 방법이다. 방대한 User x Item Matrix의 유사도를 학습함으로써, 임의의 사용자와 유사한 사용자(neighbor)들이 해당되는 아이템에 대해 내린 점수를 기반으로 유저 A의 해당 아이템 선호도를 예측하는 것이다. 이 프로젝트의 경우에는 User 대신, '골목상권'의 유사도를 측정함으로써 이 상권과 유사한 잠재력을 지닌 타 골목상권을 도출하고자 한다.   
유사도를 측정하는 방식에는 여러가지가 있지만, 대표적으로 Eucleadian Distance와 Cosine Similarity 두 방식이 있다. 단순히 Eucleadian Distance만 비슷한 것을 찾는다면 특성간의 인과관계를 유추하기 어려울 것이라 생각했다. 따라서 이 실험에서는 Cosine Similarity를 사용했는데, 그 이유는 잠재적 상권을 도출하기 위해서는, 기준 상권의 과거 상태와 벡터가 유사해야한다고 판단했기 때문이다.   

![캡처5](https://user-images.githubusercontent.com/52244004/174471991-465f421a-a5b0-4a6a-ac3c-bc1a82b7af6f.PNG)
![캡처6](https://user-images.githubusercontent.com/52244004/174471998-beeed8ef-df89-4899-8be2-53591c34d355.PNG)


###### 3-1. 기준 상권 도출  
유사도 분석을 하기 위해서는 기준점이 필요하다. 기준이 되는 골목상권의 조건은 
- Filter1 : 2021년 기준 매출 규모가 '명동길'보다 클 것 : 명동은 중국과 코로나 이슈로 직격타를 입은 대표적인 상권이다. 기준이 될 상권은 명동보다는 매출 규모가 클 필요가 있다.
- Filter2 : 2017 - 2021 5년간의 성장률이 10% 이상일 것 : 성장률을 10%로 잡은 것은, 코로나19 lockdown이라는 macro한 상황이 있었기 때문에, 10%도 고무적인 성장이라고 생각했다.
이에 따라 4개의 상권을 도출했고,  2017년의 기준상권과(a), 2018/2019/2020/2021의 대상상권(b)를 비교하게 된다.
![캡처7](https://user-images.githubusercontent.com/52244004/174472006-42134bdb-3f8f-4473-b9c5-a4e890809ee0.PNG)

###### 3-2. 결과 필터링
가령 아차산로15길 상권을 예로 들면, 총 1209개의 유사도가 2018-2021의 기간동안 년도별로 나온다. 그럼 단순히 이들 가운데 top10을 추리면 그것이 최종 예측일까? 필터의 기준이 필요하다.
- Filter 1: 각 년도별 유사도가 0.95 이상이었던 상권 
- Filter 2: 2년 이상 중복해서 등장한 골목상권
![캡처9](https://user-images.githubusercontent.com/52244004/174472045-33b1fc56-3cd3-463e-b836-21a75bbc8ee7.PNG)

![캡처8](https://user-images.githubusercontent.com/52244004/174472053-5ce8ebd5-4e56-4e26-9fca-22a55224c16c.PNG)

### 4. Verification
그렇다면 예측한 결과가 실제로 의미있는 결과일까? 예측한 상권들의 시간에 따른 203040매출이 양의 correlation을 갖는다면, 예측 결과가 의미를 갖는다고 주장할 수 있겠다.
![캡처10](https://user-images.githubusercontent.com/52244004/174472061-18e2b912-859f-49f0-9895-330561473dc6.PNG)
![캡처11](https://user-images.githubusercontent.com/52244004/174472066-2673e713-ee12-4aa1-b8d4-4cd9e5dc35a6.PNG)
![캡처12](https://user-images.githubusercontent.com/52244004/174472070-a3834240-0cff-481d-a50b-5eb61dfc44fb.PNG)
![캡처13](https://user-images.githubusercontent.com/52244004/174472071-a0e37162-d4b5-41bb-ae38-979fe5299c48.PNG)

### 5. Conclusion
