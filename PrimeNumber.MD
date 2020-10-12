# 소수 구하기 Finding prime number
---
[출처](https://53perc.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%86%8C%EC%88%98-%ED%8C%90%EB%B3%84%ED%95%98%EA%B8%B0) <- 블로그 작성자입니다.

소수(Prime number)는 1과 자기자신만을 약수로 가지는 양의 정수를 말한다. 2, 3, 5, 7, 11, 13... 등의 수이다. 어떤 수가 소수인지 판단하는 방법에 대해 알아보자.

가장 간단하게 소수의 정의로부터, 어떤 자연수 N이 소수인지를 살펴보려면

N 이 1 이면 소수가 아니다.
2 부터 N-1 까지의 자연수들로 순서대로 N을 나눠서 나누어 떨어지는 수가 하나도 없으면 N은 소수이다.
라는 간단한 검사 규칙을 얻을 수 있다.

기본적으로 이 규칙을 그대로 적용하여 코드로 옮겨보면 생각보다 매우 간단한 함수가 만들어진다.

```python
def is_prime_bad(n: int) -> bool:
    if n < 2:
        return False
    for i in range(2, n):
        if n % i is 0:
            return False
    return True
```
이 함수의 문제는 이름에서 알 수 있듯이, 그 성능이 매우 나쁘다는 점이다. 120 미만의 소수를 모두 찾아보면

```python
for i in range(120):
    if is_prime_bad(i):
        print(i)
```
제법 빠르게(?) 답을 구해주는 것 같다. 그렇지만 10만 이하에는 몇 개의 소수가 있는지 세어보는 건 어떨까?

```python
%time len([x for x in range(1, 100000) if is_prime_bad(x)])

# Wall time: 39.9 s
# 9592
```
거의 40초 가까이되는 시간이 걸렸다. 만약 "2백만 이하의 소수의 합을 구하라"는 문제를 풀려고 한다면, 제 아무리 좋은 성능의 컴퓨터를 쓰더라도 매우 긴 시간이 걸릴 수 있다.

이 함수의 효율을 좀 더 높일 방법은 없을까? 소수의 성질에 대해서 조금 더 살펴보자. 소수는 1과 자기 자신 이외에는 약수를 가지지 않는다고 하였다. 그래서 2를 제외한 모든 짝수는 2로 나눠지므로 소수가 아니다. 이 성질을 이용하면 검사해야 하는 개수를 반으로 줄일 수 있다.

```python
def is_prime_bad_too(n: int) -> bool:
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 is 0:
        return False
    for i in range(3, n, 2):
        if n % i is 0:
            return False
    return True
```
그래서 테스트를 해보면,

```python
%time len([x for x in range(1, 100000) if is_prime_bad_too(x)])


# Wall time: 21.4 s
# 9592
```   
대략 절반으로 줄였다. 하지만 이 마저도 그리 좋은 성능은 아니다. 좀 더 좋은 방법이 없을까? 만약 어떤 자연수 N이 a로 나눠진다면 N = a * b 로 표현할 수 있을 것이다. 그렇다면 N 은 a 로도 b 로도 나눠진다는 말이다. 즉 이 말은 N의 제곱근 r 을 기준으로 곱해서 N이 될 수 있는 약수들이 같은 개수로 분포한다는 말이다. 따라서 N이 소수인지를 알고 싶으면 N의 제곱근까지만 검사해보면 된다. 이 룰을 추가하는 것만으로 엄청난 시간을 줄일 수 있다.

```python
def is_prime_not_bad(n: int) -> bool:
    if n < 2:
        return False
    if n is 2:
        return True
    if n % 2 is 0:
        return False
    l = round(n ** 0.5) + 1
    for i in range(3, l, 2):
        if n % i is 0:
            return False
    return True
```
테스트를 해보자
```python
%time len([x for x in range(1, 100000) if is_prime_not_bad(x)])

# Wall time: 276 ms
# 9592
```
거의 100배나 빨라졌다!!!!

조금 더 다듬어 보자. 몇 가지 트릭을 추가한다.

10미만의 작은 소수들은 2나 3으로만 나눠지지 않으면 된다.
그 이상의 수들에 대해서는 5, 7, 9, 11, 13, 15... 등의 홀수로 나눠보면 된다. 하지만 이미 3의 배수에 대해서는 1에서 검사하기 때문에 5, 7, 11, 15,... 의 패턴으로 검사할 수 있다.
이를 이용해서 최적화한 코드는 다음과 같다. 이 코드는 실제로 프로젝트 오일러의 많은 소수 관련 문제를 풀 때 사용한 함수이다.

```python
def is_prime(n: int) -> bool:
    if n < 2:
        return False
    if n in (2, 3):
        return True
    if n % 2 is 0 or n % 3 is 0:
        return False
    if n < 9:
        return True
    k, l = 5, n**0.5
    while k <= l:
        if n % k is 0 or n % (k+2) is 0:
            return False
        k += 6
    return True
```
이 함수를 이용해서 10만 이하의 소수의 개수를 세어보면

```python
%timeit len([x for x in range(1, 100000) if is_prime(x)])

# 169 ms ± 3.47 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
앞서 작성했던 제법 괜찮은 함수보다도 30%이상 빨라졌다. 이 함수를 써서 1백만 이하의 소수의 개수를 세어보면 약 4초 가량이 걸린다.

만약 처음에 소개한 가장 나쁜(?) 성능의 코드를 C로 동일하게 구현하여 1백만 이하의 소수의 개수를 세면 시간이 얼마나 걸릴까?

```python
$ time ./p.exe
78498

real    2m14.804s
user    0m0.015s
sys     0m0.000s
```
무려 2분 14.8초가 걸렸다.

부록 : 에라토스테네스의 체
에라토스테네스의 체는 소수의 계산을 조금 더 편하게 하는 방법으로 2부터 시작하는 연속된 자연수를 미리 써 두고, 처음 나타나는 수의 배수들을 모두 지우는 식으로 나아가서 소수만 남기는 방식으로 구한다. 이 방식은 미리 소수가 나타날 범위를 한정해야 한다는 단점이 있지만, 구현도 그렇게 어렵지 않고, 성능도 매우 좋다.

```python
def primes_up_to(n: int) -> [int]:
    seive = [False, False] + [True] * (n-1)
    for (i, e) in enumerate(seive):
        if e:
            k = i * 2
            while k <= n:
                seive[k] = False
                k += i
    return [x for (x, y) in enumerate(seive) if y ]
```
이를 이용해서 10만 이하의 소수의 개수를 세면?

```python
%time len(primes_up_to(100000))

# Wall time: 83 ms
# 9592
```
근데 이 방법도 더 효율적으로 개선할 수 있다. 그것은 바로 파이썬의 슬라이스 문법을 이용한 치환이다. 파이썬의 슬라이스 문법을 이용하면 특정 범위와 스텝을 통해 구간을 정할 수 있다. 이를 이용해서 리스트 내의 불연속적인 부분집합을 매우 빠르게 치환할 수 있다. 그리고 이 치환 동작은 파이썬 처리 로직이 아닌 C 레벨에서 처리되기 때문에 매우 빠르다.

```python
def primes_up_to_good(n:int) -> [int]:
    seive = [False, False] + [True] * (n - 1)
    for k in range(2, int(n ** 0.5 + 1.5):
      if seive[k]:
        seive[k*2::k] = [False] * ((n - k) // k)
    return [x for x in range(n+1) if seive[x]]
```
그리고 10만 까지의 소수의 개수를 세면, 무려 24.6ms 라는 놀라운 성능을 보인다.

그래서, 2백만 이하의 소수의 합을 파이썬으로도 1초 이내에 구할 수 있다. (0.6초 대)

```python
%time sum(primes_up_to_good(2000000))

# Wall time: 674 ms
# 142919071801
```
보통 상한선이 정해진 범위의 소수들을 가지고 풀어야 하는 문제는 이처럼 에라토스테네스의 체를 이용해서 소수 세트를 만들어 놓고 사용하는 것이 가장 빠르고 편리하다.

다음 번에는 소인수분해를 하는 코드를 알아보도록 하겠다.



출처: https://53perc.tistory.com/entry/파이썬-소수-판별하기 [53Percent]
