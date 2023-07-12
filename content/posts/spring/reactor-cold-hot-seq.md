---
title: "[Reactor] Cold Sequence와 Hot Sequence"
date: 2023-07-12T23:55:16+09:00
categories: ["kotlin"]
tags: ["java", "spring", "webflux", "reactor", "cold sequence", "hot sequence"]
draft: false
---

# Cold와 Hot의 의미
https://reactivex.io/ “Hot” and “Cold” Observables에 대해 아래와 같이 설명되어 있다.

언제 Observable이 항목 시퀀스를 방출하기 시작합니까? Observable에 따라 다릅니다. "Hot" Observable은 생성되자마자 항목을 방출하기 시작할 수 있으므로 나중에 해당 Observable에 구독하는 모든 관찰자는 시퀀스의 중간 어딘가에서 관찰을 시작할 수 있습니다.
반면에 "Cold" Observable은 관찰자가 구독하기 전까지 항목을 방출하기를 기다리므로 이러한 관찰자는 시작부터 전체 시퀀스를 볼 수 있다는 것이 보장됩니다.

즉, Hot은 생성됨가 동시에 데이터를 방출하고, 나중에 구독한 구독자는 구독시점부터의 데이터를 받게된다.
Cold는 publisher가 발행한 데이터를 구독 시점에 처음부터 데이터를 받게된다.

# Cold Sequence
```java
Flux<String> coldFlux =
	Flux
		.fromIterable(Arrays.asList("KOREA", "JAPAN", "CHINESE"))
		.map(String::toLowerCase);

coldFlux.subscribe(country -> log.info("# Subscriber1: {}", country));
log.info("==============================");

Thread.sleep(2000L);

coldFlux.subscribe(country -> log.info("# Subscriber2: {}", country));
```
실행결과
```
# Subscriber1: korea
# Subscriber1: japapn
# Subscriber1: chinese
==============================
# Subscriber2: korea
# Subscriber2: japapn
# Subscriber2: chinese
```

실행 결과를 보면 알 수 있듯이, 언제 구독을 해도 Upstream에서 반환된 Flux는 subscirber가 모든 데이터를 받을 수 있다.

## Hot Sequence
```java
String[] singers = {
	"Singer A",
	"Singer B",
	"Singer C",
	"Singer D",
	"Singer E"
};

log.info("# Begin concert:");

Flux<String> concertFlux =
	Flux
		.fromArray(singers)
		.delayElements(Duration.ofSeconds(1)) // 1초에 1개씩 데이터 발행
		.share(); // Cold Sequnece를 Hot Sequnece로 동작하게 해주는 Operator다.

concertFlux.subscribe(
	singer -> log.info("# Subscriber1 is watching {}'s song",
								singer));

Thread.sleep(2500);

concertFlux.subscribe(
	singer -> log.info("# Subscriber2 is watching {}'s song",
								singer));

```
```
09:11:16.375 [main] INFO - # Beigin concert:
09:11:17.375 [parallel-1] INFO - # Subscriber1 is watching Singer A's song
09:11:18.375 [parallel-2] INFO - # Subscriber1 is watching Singer B's song
09:11:19.375 [parallel-3] INFO - # Subscriber1 is watching Singer C's song
09:11:19.375 [parallel-3] INFO - # Subscriber2 is watching Singer C's song
09:11:20.375 [parallel-4] INFO - # Subscriber1 is watching Singer D's song
09:11:20.375 [parallel-4] INFO - # Subscriber1 is watching Singer D's song
09:11:21.375 [parallel-1] INFO - # Subscriber1 is watching Singer E's song
09:11:21.375 [parallel-1] INFO - # Subscriber1 is watching Singer E's song
```
반환 결과를 보면 구독 시점에 발행되는 데이터를 받을 수 있다.
