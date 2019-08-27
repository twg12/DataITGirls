# 8월 20일

## 이산 확률 분포
* 이항 분포 (binomial distribution)
    * p, 1-p
    * binomial(a, b, c)

* 다항 분포 (multinomial distribution)
    * p1 + p2 + p3 = 1
    * multinomial(a, b)

* 초기하 분포 (hypergeometric distribution)
    * s1, s2가 있는 샘플 n개 뽑을 때의 확률
    * without replacement

* 기하 분포 (geometric distribution)
    * 성공 = p, 얼마나 시도할 것인가
    * geometric(a, b) : 이탈률이 a일때 b횟수는 몇번 이탈할 것인가

* 음이항 분포 (negative binomial distribution)
    * p를 n번 성공할 때까지 시도했을 때 시도 횟수
    * negative_binomial(a, b)

* 포아송 분포 (poisson distribution)
    * 평균 L 일어나는 일, X번 일어날 확률의 분포
    * poisson(a, b) : 평균 a일때 100번 일어날 확률

## 연속 확률 분포
* 균등 분포
    * (m, s)
    * uniform(a, b, c) 
* 정규 분포
    * (m, s) 
    * normal(a, b, c)
* 지수 분포
    * 평균적으로 B 시간마다 일어나는 일: 다음 일 언제?
    * exponential(a)
