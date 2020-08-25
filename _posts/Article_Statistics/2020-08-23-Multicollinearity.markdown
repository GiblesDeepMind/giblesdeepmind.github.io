---

title:  "[다중공선성] 살면서 한번쯤은 마주하는 다중공선성"
excerpt: "다중공선성"

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

#classes: wide

# layout: posts

date:   2020-08-24 01:21:25 +0900
categories: 
  - Statistics
tags: 
  - Multicollinearity
  - 다중공선성
  - 다중공선성 회귀분석
  - 회귀분석 다중공선성
#last_modified_at: 2020-07-19T00:40:00-05:00

# 포스팅 마크다운 상단 최상단에 표시
star: true 
use_math: true

---



# 1. 어디로 가야하오?
![](https://giblesdeepmind.github.io/assets/images/Statistics/Multicollinearity/leesin.png){: .align-center}

- 다중공선성을 만난 우리들

본 글에서는 "어디로 가야하오" 에 담긴 두 가지 의미를 다룰 것이다.

1. 다중공선성을 꼭 파악해야하는가?
2. 반드시 해결해야하는 문제인가?

---
# 2. 회귀분석 A씨와 다중공선성 B씨의 소개팅

회귀분석 A씨는 들뜬 마음으로 다중공선성 B씨와의 소개팅을 준비중이었다. 그러다 문득 이런 생각이 들었다. 

"남들도 다 한번쯤은 만난다고해서 만나기는 하는데 괜히 시간만 버리면 어쩌지?"

A씨 생각대로 정말 시간 낭비일까?

## 2-1. 다중공선성B씨의 사정

B씨는 메갓 선수의 퍼포먼스를 매 경기마다 측정하는 퍼포먼스 분석가이다. 

어느날 B씨는 수집한 데이터를 기반으로 지난 1년간 메갓 선수의 숏 패스 / 롱 패스 / 쓰루 패스 성공률 추이를 살펴봤다.

분석 결과 각 변수들 간의 상관이 매우 높은 것으로 나타났다. 즉, 숏 패스의 성공률이 저조하면 롱 패스와 쓰루 패스의 성공률도 저조하고 롱 패스의 성공률이 높으면 숏 패스와 쓰루 패스의 성공률도 높다는 사실을 발견했다.

B씨에게는 매우 의미있는 행위이다. 왜냐하면, 세 가지 패스가 상관이 있다고 해서 한 가지 패스만을 측정할 수는 없기 때문이다. 숏 패스, 롱 패스, 쓰루 패스는 각각 고유한 의미를 가진다.

하지만, 다른 사람들의 생각은 다르다. "비슷한 정보를 가지고 있으면 하나만 사용하는게 낫지 않은가?"

## 2-2. 회귀 계수의 신뢰 구간

다중공선성은 B씨의 분석 결과와 같이 여러 변수들이 비슷한 정보를 공유하는 현상을 말한다. 

![](https://giblesdeepmind.github.io/assets/images/Statistics/Multicollinearity/diagram.JPG){: .align-center}

이제 다중공선성이 무엇인지 대략적인 감을 잡았을 것이다. 

본격적으로 들어가기전에 다음 질문에 대해서 한번 생각해보자.

"회귀 모델을 어떻게 신뢰할 수 있는가?" 

다들 각자만의 생각이 있겠지만,
- $R^2$가 높은게 짱이야!
- 아니야 AIC랑 BIC가 낮아야 해
- etc

이 문제에 대해서 원초적으로 생각해보자. 

회귀 분석에는 아웃풋을 도출하는 명확한 함수식이 존재한다. 그리고, 우리는 이 함수식을 통해 어떤 결론을 내리게 된다. 

아래와 같은 가상의 모델을 가정해보자.

$Y= 0.7X_1 + -0.3X_2 + 0.2$

- Y : 실점 횟수
- $X_1$ : 스트레스
- $X_2$ : 수비훈련기간

모델에서 스트레스 점수가 높을수록 실점 횟수가 증가하지만 수비훈련기간에 따라 그 영향력이 경감될 수 있음을 알 수 있다.

하지만, 여기서 끝이 아니다. **회귀 계수가 안정적이어야만 위와 같은 결론을 내릴 수 있다.** 따라서 회귀 계수 신뢰 구간을 확인할 필요가 있다.

만약, X1 변수의 회귀 계수 $\beta_2$의 신뢰 구간이 다음과 같다고 가정해보자.

$-0.3 - 0.4 <\beta_2 < -0.3 + 0.4$

여기서 중요한 포인트는 두 가지 이다.

* 회귀 계수의 신뢰 구간에 0이 포함된다.
  * $\hat{\beta} 값이 0일 수 있다.$
  * 즉, 변수가 유의미하지 않다.
* 회귀 계수가 +가 될 수도 있다.
  * 해석의 방향이 반대가 될 수 있다.

이처럼 신뢰 구간이 넓어지면 계수 해석 방향이 달라 질 수 있다.

## 2-3. 다중공선성과 회귀 계수의 신뢰 구간

**<span style="color:red">다중공선성은 회귀 계수의 신뢰 구간을 넓힐 수 있다.</span>** 

그렇기 때문에 우리는 시간을 투자하여 다중공선성을 살펴봐야 한다.

$\hat{\beta_2}$의 분산식은 다음과 같다.

$Var[\hat{\beta_2}]=\sigma^2({ X }^{ T }X)^{-1}$ 

$\sigma^2$는 오차항의 분산이라 MSE로 추정한다. 

$({ X }^{ T }X)^{-1}$에 대해 생각하기전에 $X$가 다음과 같은 Matrix라고 가정해보자. 

$$X=\begin{pmatrix} 1&1&0 \\ 0&1&1 \\ 0&0&0 \end{pmatrix}$$

두 번재 열은 첫 번째 열과 세 번째 열의 선형 결합으로 표현된다. 이렇게 될 경우 ${ X }^{ T }X$의 역행렬이 존재하지 않게 된다. 

역행렬이 존재 하지 않는 다는 것은 바꿔 얘기하면 Matrix안에 Free variable이 있다는 뜻이고 그로 인해 유일해가 존재하지 않는다는 의미가 된다. 

자세한 내용은 아래 페이지를 참고하면 좋을 것 같다.
> [Full Column Rank](https://twlab.tistory.com/22)

위 경우는 매우 극단적인 경우이다. 하지만, 위와 같은 경우가 아니더라도 한 변수가 나머지 변수들과 매우 흡사한 경우가 있을 수 있다.

이때, $({ X }^{ T }X)^{-1}$의 역행렬은 다음과 같이 계산된다.

$({ X }^{ T }X)^{-1} = \frac{1}{det(A)}adj(A)$

**여기서 $det(A)$가 0에 가까워질수록(=선형종속적인 성격을 띌수록) $({ X }^{ T }X)^{-1}$의 값이 커지게 되고 $Var[\hat{\beta_2}]$이 커지게 된다.**

> $det(A)$가 0에 가까워질수록?
>> 역행렬 존재 조건이 $det(A) = 0$이라는 것을 생각해보자. 앞서 얘기했듯이 한 열이 다른 열들의 선형 결합으로 표현될 경우 역행렬이 존재하지 않게 된다. 이 말을 바꿔서 생각하면 다른 열들의 선형 결합이 한 열을 근사하게 표현할 경우 $det(A)$값이 0에 가까워진다는 의미가 된다. 

---
# 3. 다중공선성은 정말로 위협적인 존재인가?


앞서 다중공선성으로 인해 회귀 계수의 신뢰 구간이 넓어질 수 있다는 얘기를 했다.

이번 챕터에서는 우리가 다중공선성으로 인해 실질적으로 영향을 받을 수 있는지에 대해 두 가지 관점에서 알아볼 것이다.

1. 예측(모델 성능)
2. 해석(추론)

## 3-1. 다중공선성은 예측 성능에 어떤 영향을 미치는가? 

결론부터 얘기하면 예측 성능 자체에는 큰 영향을 미치지 않을 수 있다. 

여기서 예측 성능이란 예측값을 뜻한다. 그림을 통해 살펴보자.

![](https://giblesdeepmind.github.io/assets/images/Statistics/Multicollinearity/Multi_space.JPG){: .align-center}
> Applied Linear Statistical Models
>> 다중공선성이 존재할 때 해 공간 예시

상단의 식 $X_2 = 5 + .5X_1$은 $X_2$과 $X_1$의 상관이 높다는 것을 의미한다.

그림에서 해집합을 표현할 수 있는 평면은 2개(그 이상일 수 있음)이지만 해집합 자체(빨간선)는 하나이다. 

다시 말하자면 공선성이 발생하여 회귀 계수가 넓은 신뢰 구간을 가질 때, 하나의 값을 추정하는 여러 회귀식이 존재할 수 있다.

### 정말 예측 성능에는 영향을 미치지 않을까?

다중공선성이 예측 성능에 큰 영향을 미치지 않는다고 말하려면 한 가지 전제가 더 필요하다.

ML 모델에서 데이터는 크게 훈련/테스트 셋으로 나누어진다.

만약, 데이터 수가 너무 적어 훈련 셋과 테스트 셋 간에 공분산 구조가 다르다면 다중공선성은 또 다른 문제를 야기할 수 있을 것이다.

> 공분산 구조가 다르다는 것은 다음의 의미이다. 예를 들어, 샘플 수가 13개 밖에 안되고 이 중 4명은 남자 9명은 여자인 데이터 셋을 가정해보자. 이때, 훈련 셋과 테스트 셋으로 데이터를 분할할 경우 테스트 셋에 모든 남자가 포함될 수도 있다.
>> 이럴 경우 모델이 여자에 대한 정보만 학습하고 추론은 남자에 대한 정보를 바탕으로 하게되는 상황이 발생할 수 있다.
>>>즉, 훈련 셋에서 반영된 변수 간 다중공선성 정도와 테스트 셋에서 반영될 다중공선성 정도가 다를 수 있다.
>>>> 그리고 이와 같은 차이는 훈련 셋과 테스트 셋 간 공분산 구조가 다를 경우 발생할 수 있다.

결론적으로 훈련 셋과 테스트 셋 간 공분산 구조에 큰 차이가 없을 경우
>(훈련 셋에서 발생하는 다중공선성과 테스트 셋에서 발생하는 다중 공선성에 큰 차이가 없는 경우) 

다중공선성은 모델의 예측 능력에 큰 영향을 미치지 않을 수 있다.

### 사례. 다중공선성을 지닌 데이터를 의사결정나무에 적용하는 경우

Piramuthu(2008)는 데이터에 아웃라이어가 존재하는 경우 / 이상치가 존재하는 경우 / 다중공선성이 있는 경우 등으로 조건을 나누어 의사결정나무에 어떤 영향을 미치는지를 확인하는 시뮬레이션 연구 결과를 진행했다.

시뮬레이션 결과 다중공선성이 있는 변수를 제거했을 때 오히려 모델 성능이 하락한 것을 확인할 수 있다.

![](https://giblesdeepmind.github.io/assets/images/Statistics/Multicollinearity/prediction_error_tree.JPG){: .align-center}

> 다른 변수들과 상관이 있는 $x_4$를 제거했을 때 예측 오류가 더 높아짐

물론, 위 사례를 모든 경우에 일반화 할 수는 없다. 하지만, 다음의 사실을 시사하기에는 충분한 사례라고 생각한다.

* 모델 성능 향상이 목적인 경우 두었을 때 다중공선성 만으로 변수를 제거하면 오히려 성능이 떨어질 수도 있다. 

## 3-2. 다중공선성은 모델 해석에 어떤 영향을 미치는가?

2-2에서 살펴봤듯이 다중공선성이 발생하면 회귀 계수의 신뢰 구간이 넓어진다. 같은 사례로 이 내용을 한번 더 복기해보자. 

$Y= 0.7X_1 + -0.3X_2 + 0.2$

- Y : 실점 횟수
- $X_1$ : 스트레스
- $X_2$ : 수비훈련기간

$-0.3 - 0.4 <\beta_2 < -0.3 + 0.4$

당신은 토트넘의 감독 무리뉴다. 팀 분석가한테 `요즘 골이 많이 먹히는 것 같아`라고 고민을 말했더니 분석가가 위와 같은 모델을 들고 왔다.

모델을 보고 만족한 무리뉴는 분석가에게 `약팀이랑 붙었던 경기는 빼고 다시 결과를 말해줘`라고 부탁했다. 

그랬더니, $Y= 0.8X_1 + 0.2X_2 + 0.3$ 와 같은 모델이 나왔다. 

이때, 당신은 분석가에게 뭐라고 할 것인가?

1. $\beta_2$의 계수가 들쑥날쑥 하는군 역시 축구의 세계는 알다가도 모르는 것이야
2. 훈련이 별로 효과가 없나봐
3. 그동안 정말 고생이 많았네. 2만원을 줄테니 오늘 저녁은 치킨을 먹고 내일부터는 안나와도 된다네

선택은 이 글을 읽는 분들께 맡기겠다.

다시 한번 얘기하지만, 다중공선성이 발생하면 회귀 계수의 신뢰 구간이 넓어진다. 

이는 두 가지 문제를 초래한다.

1. 신뢰 구간이 넓을 수록 회귀 계수를 신뢰할 수 없다.(=회귀 계수를 이용해 판단을 하기 어렵다)
2. 모델의 성능이 좋아도 성능의 원인을 명확히 파악하기 어렵다.(비슷한 특징을 가진 여러 변수들이 모델에 존재하기 때문에)

### 다중공선성을 제거하면 정말 모델에 대한 해석력이 좋아지는가?



---
# Reference
* [Collinearity](https://genomicsclass.github.io/book/pages/collinearity.html)
* [Multicollinearity: Why does highly correlated columns in the design matrix lead to high variance of the regression coefficient?](https://math.stackexchange.com/questions/1178431/multicollinearity-why-does-highly-correlated-columns-in-the-design-matrix-lead)
* [Why do we use the term multicollinearity, when the vectors representing two variables are never truly collinear?](https://stats.stackexchange.com/questions/128735/why-do-we-use-the-term-multicollinearity-when-the-vectors-representing-two-vari)
* [Lecture 17: Multicollinearity](http://www.stat.cmu.edu/~larry/=stat401/lecture-17.pdf)
* [Is multicollinearity really a problem?](https://stats.stackexchange.com/questions/268966/is-multicollinearity-really-a-problem)
* [Linear Models: How does multicollinearity affect prediction accuracy?](https://www.quora.com/Linear-Models-How-does-multicollinearity-affect-prediction-accuracy)
* [Multicollinearity and predictive performance](https://stats.stackexchange.com/questions/361247/multicollinearity-and-predictive-performance)
* Kutner, M. H., Nachtsheim, C. J., Neter, J., & Li, W. (2005). Applied linear statistical models (Vol. 5). New York: McGraw-Hill Irwin.
* Piramuthu, S. (2008). Input data for decision trees. Expert Systems with applications, 34(2), 1220-1226.