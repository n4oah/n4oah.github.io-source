---
title: "[Kotlin] Object 키워드 활용하기"
date: 2021-01-08T10:53:16+09:00
categories: ["kotlin"]
tags: ["kotlin", "kotlin object", "keyword", "코틀린"]
draft: false
---
***

## Kotlin Object

코틀린에는 static 키워드가 없기 때문에 object 키워드를 활용하여 static을 대체할 수 있어야 함.  
코틀린에서는 object로 static키워드를 바이트코드 변환시 best practice적인 방법으로 제공해줌 (Java로 디컴파일러 해보면 의도를 알 수 있음)

> object 키워드는 static이 아닙니다

***

### Overview

- [Kotlin Object](#kotlin-object)
  - [Overview](#overview)
  - [싱글턴 class 만들기](#싱글턴-class-만들기)
  - [static 메소드 만들기 companion object](#static-메소드-만들기-companion-object)
  - [익명객체 만들기](#익명객체-만들기)

***

### 싱글턴 class 만들기

아래 코드는 학생들의 평균 점수를 구하는 메소드를 구현한 싱글턴 클래스 입니다.  
object객체는 Java코드로 디컴파일시 class로 변환되므로 extends, implements를 구현할 수 있습니다.

```kotlin
object StudentManager {
  val students = arrayListOf()

  fun calculateAvg(): Unit {
    for (student in students) {
      ...
    }
  }
}
```

object 키워드로 생성된 object에 접근하려면 object의 '이름.요소'로 접근하면 됩니다.

```kotlin
StudentManager.students.add(...);
StudentManager.calculateAvg();
```

***

### static 메소드 만들기 companion object

static을 사용할 수 없는 대신 companion object로 top-level function을 만들 수 있습니다.
> companion object는 private 맴버에는 접근 할 수 없습니다.

```kotlin
class Foo {
  companion object {
    fun bar(): Unit {
      ...
    }
  }
}
```

아래와 같이 호출 가능

```kotlin
Foo.bar();
```

companion object에 이름도 정할 수 있습니다.

```kotlin
class Foo {
  companion object Name {
    fun bar(): Unit {
      ...
    }
  }
}

// 둘 다 호출 가능
Foo.Name.bar();
Foo.bar();
```

***

### 익명객체 만들기

클릭 리스너가 onClick이벤트를 발생하였을 때 익명으로 생성 된 function이 실행되는 것을 확인할 수 있습니다.

```kotlin
interface ClickListener {
  fun onClick()
}

fun main(args: Array) {
  setClickAction(object: ClickListener {
    override fun onClick() {
      ...
    }
  })
}
  
fun setClickAction(clickListener: ClickListener) {
  clickListener.onClick()
}
```
