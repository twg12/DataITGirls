## 통계: '추정'

# 선형모형
* MSE: numpy.mean((data - 예측) ** 2)
* from scipy.optimize import minimize
```py
def mse(m):
    return numpy.mean((data - m) ** 2)

minimize(mse, n)

# absolute value differences

def mae(m):
    return numpy.mean(numpy.abs(data - m))

minimize(mae, n)
```
* 총 함수
```py
def mse(param):
    a, b = param
    예측 = 독립변수 * a + b
    return numpy.mean((종속변수 - 예측) ** 2)
minimize(mse, (a, b))
```
* 리스트를 적을 때 numpy.array([a, b, c, d]) 적을 것

# 로지스틱 선형모형
* numpy.log(numpy.product(like))
```py
def lk(p):
    like = binom.pmf(data, n, p)
    return -numpy.sum(numpy.log(like))

성공확률 = expit(a * 독립변수 + b)
성공우도 = numpy.log(성공확률) * 성공
실패우도 = numpy.log(1-합격확률) * (1-합격)
우도 = 성공우도 + 실패우도
```