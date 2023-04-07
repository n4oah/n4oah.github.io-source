---
layout: post
title: "[C#] Call By Reference"
date: 2020-06-24T10:53:16+09:00
categories: ["c-sharp"]
tags: ["C#", "call by reference"]
draft: false
---

***

## Call by Reference와 Call By Value

C#에서 메소드를 호출할 때 파라미터 값을 넘겨야 할 때가 있습니다.  
메소드가 실행 될 때 값을 그대로 복사하여 메소드 종료 이후 stack상에서 사라지는 파라미터를 call by value, 복사가 아닌 메모리 주소가 넘겨지는 방식을 call by reference라고 합니다.  

> C언어의 포인터가 call by reference라고 보시면 이해하기 훨씬 쉽습니다.

***

## Overview

- [Call by Reference와 Call By Value](#call-by-reference와-call-by-value)
- [Overview](#overview)
- [C#에서의 Call By Value](#c에서의-call-by-value)
- [C#에서의 Call By Reference](#c에서의-call-by-reference)
- [만약 ref키워드가 없는 자바같은 언어라면?](#만약-ref키워드가-없는-자바같은-언어라면)

***

## C#에서의 Call By Value

Swap 메소드는 파라미터 a와 b의 값을 바꾸어 주는 역할을 하는 것 처럼 보입니다.

```c#
static void Main (string[] args) {
    int a = 10,
        b = 20;

    Console.WriteLine("Swap메소드를 실행하기 전 : a = " + a + ", b = " + b);

    Swap(a, b);

    Console.WriteLine("Swap메소드를 실행한 뒤 : a = " + a + ", b = " + b);
}

static void Swap (int a, int b) {
    int temp;

    temp = a;
    a = b;
    b = temp;
}
```

위의 코드의 output

```sh
Swap메소드를 실행하기 전 : a = 10, b = 20
Swap메소드를 실행한 뒤 : a = 10, b = 20
```

결과를 보면 알 수 있드시 파라미터로 넘어간 값의 주소와 실제 메소드에서 사용한 메모리의 주소가 다르다는 것을 유추 할 수 있습니다.

***

## C#에서의 Call By Reference

위와 비슷한 소스코드 처럼 보이지만 메소드의 파라미터 자료형 앞에 ref키워드가 붙은 것을 확인 할 수 있습니다.

```note
static void Main (string[] args) {
    int a = 10,
        b = 20;

    Console.WriteLine("Swap메소드를 실행하기 전 : a = " + a + ", b = " + b);

    Swap(ref a, ref b);

    Console.WriteLine("Swap메소드를 실행한 뒤 : a = " + a + ", b = " + b);
}

static void Swap (ref int a, ref int b) {
    int temp;

    temp = a;
    a = b;
    b = temp;
}

```

위의 코드의 output

```note
Swap메소드를 실행하기 전 : a = 10, b = 20
Swap메소드를 실행한 뒤 : a = 20, b = 10
```

위와 같이 ref키워드를 파라미터에 적어주면 변수의 값이 아닌 메모리 주소가 보내지는 것을 확인 할 수 있습니다.  

***

## 만약 ref키워드가 없는 자바같은 언어라면?

C#은 ref키워드로 파라미터의 메모리 주소를 넘겨주고, 메모리 주소를 메소드안에서 변경하는 것 까지 가능했습니다.  
그러나 ref키워드가 없는 Java, Javascript같은 언어에서는 `primitive type`은 무조건 `call by value`로 파라미터가 넘겨지고 `reference type`은 복사가 아닌 메모리주소가 파라미터로 넘겨집니다.  
단, C#의 ref키워드 처럼 메모리 주소를 변경하는 것 까진 불가능 하고 reference의 하위 필드들의 값을 변경하는 것은 가능합니다. *(C#도 ref, out키워드를 사용하지 않으면 똑같은 성격임)*
