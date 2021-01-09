---
layout: post
title: "[javascript] 꼬리 물기 최적화 (Tail Call Optimization)"
date: 2020-06-24T10:53:16+09:00
categories: ["javascript"]
tags: ["javascript", "js", "tail call optimization", "꼬리물기 최적화"]
draft: true
---

***

## 꼬리 물기 최적화 (Tail Call Optimization)

함수 안에서 함수를 호출하면 `호출이 된 함수`에서 `호출을 한 함수`로 돌아오는 반환 지점을 가지고 있어야 한다.  
Tail Call Optimization는 `호출을 한 함수`로 돌아오지 않아도 되도록 함수 설계하여 call stack메모리를 계속 재활용 할 수 있도록 하는 것 이다.  
> 최종적으로, 마지막 호출이 된 함수가 최초로 호출 된 함수로 반환

***

## 조건

- 맨 마지막 구문에서 함수를 실행해야 함
- 언어 스팩상 `호출을 한 함수`의 stack에 메모리를 쌓지 않는 연산자는 사용 가능함

***

## Tail Recursion 피보나치 수열 구하기

정확히 100번의 재귀호출로 피보나치 수열을 구하는 함수이다

```js
function fibonacci(n, prevFibo = 1, prevPrevFibo = 0) {
  if (n < 2) {
    return prevFibo;
  }

  return fibonacci(n - 1, prevFibo + prevPrevFibo, prevFibo);
}
```

5번 째의 피보나치 수열을 구해보자

```js
fibonacci(5); // 5

```

함수는 내부적으로 아래와 같이 동작되어 값을 반환할 것이다

```note
call fibonacci(5)
파라미터 값을 (4, 1, 1)로 변경
파라미터 값을 (3, 2, 1)로 변경
파라미터 값을 (2, 3, 2)로 변경
파라미터 값을 (1, 5, 3)로 변경
return 5
```

즉, 리턴은 재귀의 맨 마지막 호출일 경우에만 하므로, call stack을 최적화 시킬 수 있다

***

## 언어 정리

- Tail Recursion: Tail Call을 재귀함수로 사용할 때 Tail Recursion라고 함

***

## 기타 참고사항

- Javascript의 언어 스팩상 삼항 연산자는 call stack에 메모리를 생성하지 않음
- 후위 증감 연산자는 call stack에 메모리를 생성함
- 브라우저별 tail call optimization 지원 현황을 확인하려면 [여기](https://kangax.github.io/compat-table/es6/){:target="_blank"}{:class="a-tag-link"}를 클릭하세요
- 2020/06/24일 기준 tail call optimization을 지원하는 브라우저는 사파리밖에 없음

## 참고

- [https://homoefficio.github.io/2015/07/27/%EC%9E%AC%EA%B7%80-%EB%B0%98%EB%B3%B5-Tail-Recursion/](https://homoefficio.github.io/2015/07/27/%EC%9E%AC%EA%B7%80-%EB%B0%98%EB%B3%B5-Tail-Recursion/){:target="_blank"}
- [https://velog.io/@yesdoing/%EA%BC%AC%EB%A6%AC-%EB%AC%BC%EA%B8%B0-%EC%B5%9C%EC%A0%81%ED%99%94Tail-Call-Optimization%EB%9E%80-2yjnslo7sr#-%EA%B7%B8%EB%9F%BC-%EC%99%9C-%EA%BC%AC%EB%A6%AC-%EB%AC%BC%EA%B8%B0-%EC%B5%9C%EC%A0%81%ED%99%94%EB%A5%BC--%ED%95%A0%EA%B9%8C%EC%9A%94](https://velog.io/@yesdoing/%EA%BC%AC%EB%A6%AC-%EB%AC%BC%EA%B8%B0-%EC%B5%9C%EC%A0%81%ED%99%94Tail-Call-Optimization%EB%9E%80-2yjnslo7sr#-%EA%B7%B8%EB%9F%BC-%EC%99%9C-%EA%BC%AC%EB%A6%AC-%EB%AC%BC%EA%B8%B0-%EC%B5%9C%EC%A0%81%ED%99%94%EB%A5%BC--%ED%95%A0%EA%B9%8C%EC%9A%94){:target="_blank"}
