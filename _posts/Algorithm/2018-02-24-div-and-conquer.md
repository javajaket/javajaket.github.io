---
layout: post
title: 'Divide-and-conquer 알고리즘'
excerpt: 분할 정복 알고리즘에 대해 알아본다.
category: Algorithm
tags:
  - Algorithm
  - divide-and-conquer
---

## 분할 정복 알고리즘 (Divide-and-conquer)

> ### 분할 정복 알고리즘
> 분할 정복 알고리즘(Divide and conquer algorithm)은 그대로 해결할 수 없는 문제를 작은 문제로 분할하여 문제를 해결하는 방법이나 알고리즘이다.
> 빠른 정렬이나 합병 정렬로 대표되는 정렬 알고리즘 문제와 고속 푸리에 변환(FFT) 문제가 대표적이다.
> 출처: [위키피디아](https://ko.wikipedia.org/wiki/%EB%B6%84%ED%95%A0_%EC%A0%95%EB%B3%B5_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

분할 정복은 한번에 해결할 수 없는 문제를 한번에 해결할 수 있는 가장 작은 단위의 문제로 분할시켜 해결하는 재귀적 풀이 방법을 말한다. 아래와 같은 두 가지 단계를 가진다.  

1. 기본 단위 문제를 해결한다. 가능한 간단한 문제이어야 한다.
2. 기본 단위 문제가 될 때까지 문제를 나눈다.  

- - -

### 분할 정복 적용 예 

다음의 배열이 있다고 하자.

```py
[2, 4, 6]
```

이 배열 내의 숫자들의 총 합을 구하는 함수를 아래와 같이 만들 수 있다.  

```py
def sum(arr):
    total = 0
    for x in arr:
        total += x
    return total
```

이를 분할 정복을 적용한 방법으로 구현해보자.  

- - -

#### 1단계

먼저 가장 간단히 해결할 수 있는 기본 단위 문제를 찾는다.  

이 경우 배열의 길이가 0 이거나 1 인 경우가 가장 간단히 해결할 수 있는 문제이다.  

- 배열의 길이가 0 이면 합계는 0
- 배열의 길이가 1 이면 합계는 그 수 자체와 동일  

이를 sum 함수에 반영하면 아래와 같이 될 것이다.

```py
def sum(arr):
    if len(arr) == 0:
        return 0
    elif len(arr) == 1:
        return arr[0]
    else:
        total = 0
        for x in arr:
            total += x
        return total
```

- - -

#### 2단계

문제를 여러 개의 기본 단위 문제가 되도록 분할한다.  
먼저 2, 4, 6 의 합을 구하는 방법은 아래와 같이 분할 될 수 있다.

```py
sum([2, 4, 6]) -> 2 + sum([4, 6])
```

2, 4, 6 의 합을 구하는 것은 2 에 `sum([4, 6])`의 결과 더하는 것으로 분할할 수 있다.

이렇게하면 sum 함수에 더 짧은 배열을 넘기게 되어 문제의 크기를 줄이게 된다.  

다시 4와 6의 합을 구하는 것은 4 에 `sum([6])`의 결과를 더하는 것으로 분할할 수 있다.

```py
sum([4, 6]) -> 4 + sum([6])
```  

`sum([6])`은 1단계에서 정의한 가장 간단히 해결할 수 있는 기본 단위 문제이다.

- - -

#### 정리

지금까지의 과정을 정리해보면 sum 함수를 어떤 배열을 받았을 때 첫 번째 숫자와 나머지 숫자들의 sum 결과를 더하여 리턴하는 재귀 함수로 정의 할 수 있다.

```py
def sum(arr):
    if len(arr) == 0:
        return 0
    elif len(arr) == 1:
        return arr[0]
    else:
        first = arr.pop(0)
        return first + sum(arr)
```

어떤 배열을 받으면 길이가 1인 배열이 전달될 때까지 sum 함수를 반복 실행하게 된다.  

`[2, 4, 6]` 을 입력한 경우 sum 함수에 `[6]`이 전달될 때까지 sum 함수가 반복 호출된다.  

그러다 `[6]` 이 전달되면 sum 함수의 실행없이 바로 `6`을 리턴하게 되고 그때부터 차례로 `first + sum(arr)` 부분을 수행하게 된다.  

가장 마지막으로 실행된 sum 함수가 `6` 을 리턴하면 바로 그 전에 실행된 함수가 `first + sum(arr)` 즉, `4 + 6` 을 계산하고 이 결과를 가장 처음 실행된 함수의 `first + sum(arr)` 의 `sum(arr)` 자리에 돌려주게 된다.  
그러면 가장 처음 실행된 함수는 `2 + 10` 을 계산하여 최종적으로 `12` 를 리턴하게 된다.  

1. `sum([2, 4, 6])`
2. `2 + sum([4, 6])`
3. `2 + 4 + sum([6])`
4. `2 + 4 + 6`
5. `2 + 10`
6. `12`

- - -

#### Reference

- Hello Coding 그림으로 개념을 이해하는 알고리즘, *한빛미디어*