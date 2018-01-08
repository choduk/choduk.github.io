---
title: "Java Garbage Collection (2)"
date: 2018-01-07
categories:
 - Develop
 - JAVA
tags:
 - GC
---

#### Old GC
- Serial GC
- Parallel GC
- Parallel Old GC
- CMS GC (Concurrent Mark & Sweep GC)
- G1 GC (Garbage First GC)

---
#### Serial GC
![Serial GC](https://image.slidesharecdn.com/javaoptimizationtwjug-131222104941-phpapp01/95/introduction-of-java-gc-tuning-and-java-java-mission-control-60-1024.jpg?cb=1387709567)
- Young 영역에서는 기본적인 GC 동작방식을 그대로 따름
- Old 영역에 대해서 mark-sweep-compact 알고리즘을 이용
- 해당 GC는 메모리가 적고 적은수의 CPU에서만 사용하기 적합
- 활성화 옵션 -> `-XX:+UseSerialGC`

> Serial Collector의 한계 
> 애플리케이션이 거대해짐에 따라 Heap 메모리가 함께커지므로  Serial Collector 성능의 한계가 나타나게 됨 (Full GC로 인한 suspend)
>   - Throughput Collector -> 병렬처리를 통해 통해 성능 개선
>   - Low Pause Collector -> suspend를 분산시켜 STW 시간을 줄이는 방법

---
#### Parallel GC
![Serial GC vs Parallel GC](https://image.slidesharecdn.com/javaoptimizationtwjug-131222104941-phpapp01/95/introduction-of-java-gc-tuning-and-java-java-mission-control-63-1024.jpg?cb=1387709567)
- Throughput Collector 라고도 불림
- JDK 4에 추가됨
- Serial GC와 동작방식이 같으나, Multi-Thread가 동시에 Garbage Collection을 수행 (Parallel Copy Algorithm)
- Young 영역에서만 동작함 (`Minor GC`)
- 활성화 옵션 -> `-XX:+UseParallelGC`

---
#### Parallel Old GC 
![Parallel Old GC](http://i0.wp.com/appchemist.net/wp-content/uploads/2013/04/7.png?zoom=2&resize=699%2C602)
- Parallel Compaction Collector 라고도 불림
- JDK 5 Update6 에 추가됐으며, JDK 6부터 디폴트 방식이 됨
- Parallel Collector에 Old 영역 GC(`Major GC`) 방식이 변경(Mark-Sweep-Compaction -> Mark-Summary-Compaction) 
	1. Mark
		- Old 영역을 `Region` 단위로 균일하게 나눔 (`Region` 은 논리적인 구역이다, 보통 2kbytes 단위)
		- `STW`가 발생하며  Parallel하게 각 Thread가 Live Object를 Marking
		- Live Object의 size 및 위치 정보 등을 갱신함
	2. Summary
		- 각 `Region` Live Object 밀도를 평가하여, `Dense prefix`를 설정
		- `Dense prefix`를 기준으로 왼쪽은 GC대상에서 제외되고, 오른쪽은 GC / Compaction 대상
		- 즉, `Dense prefix`란, 위에서 측정한 밀도정보를 바탕으로 어느 `Region`을 GC / Compaction 대상으로 삼을지 정하는 작업을 뜻함
	3. Compaction
		- `STW`발생
		- Summary에서 GC 대상이된 `Region`을 담당하는 Thread 들이 수행
		- Source Region 을 제외한 나머지 Region에서 Sweep를 수행하여, GC를 수행 (Source Region이란, Compaction 대상이 되는 주로 우측에 있는 Region들)
		- Destination Region들이 Sweep 된 이후, Source Region에서 Live Object 들을 Destination Region에 옮김
- 활성화 옵션 -> `-XX:+UseParallelOldGC`

---
#### CMS GC (Concurrent Mark & Sweep GC)
![CMS GC](http://d2.naver.com/content/images/2015/06/helloworld-1329-5.png)
- Low Pause Collector 의 일종으로, suspend time을 분산하여 응답시간을 개선한 방식으로, Low Latency GC 라고도 불림
- Young 영역에서는 Parallel GC와 동일 (Parallel Copy Algorithm)
- Old 영역에서는 Concurrent Mark-Sweep Algorithm (Initial Mark, Concurrent Mark, Remark, Concurrent Sweep)
	1. Initial Mark
		- `STW`가 발생하며, Single Thread에서 수행
		- `Root set`에서 Live Object 만 빠르게 Mark
		- 전체 Object를 탐색하는 것이 아니기 때문에 굉장히 짧음
	2. Concurrent Mark
		- Initial Mark에서 발견된 Live Object 들이 참조하는 다른 Object들을 찾아서 Mark
		- `STW`가 발생하지 않고, Concurrent하게 수행됨
		- 때문에 해당 작업 도중 새로운 객체가 생성될 수 있음
	3. Remark
		- `STW`가 발생하며, Multi-Thread에서 수행
		- Concurrent Mark 과정중 생성된 객체 및 찾아낸 모든 Live Object들이 아직 Reachability한지 탐색
	4. Concurrent Sweep
		- 각 Thread에서 Sweep를 수행
- 주의할점
	- Compacting 단계가 없기 때문에, Memory Fragmentation가 발생 할 수 있음
	-  Memory Fragmentation 보완방법
		- Heap을 Size를 크게 잡는다 -> `Major GC`의 시간이 늘어나게됨
		- `Free List`를 사용 :  promotion된 object의 크기와 비슷한 free space를 탐색하여 배치하는 방법이지만, 적절한 배치를 위한 Overhead가 발생
- 활성화 옵션 -> `-XX:+UseConcMarkSweepGC`
	- `Minor GC`디폴트값 -> `-XX:+UseParNewGC`

---
#### G1 GC (Garbage First GC)
![G1 GC](http://cfile28.uf.tistory.com/image/2137CF34520A29C20E4B3D)
- JDK 7 update4 부터 지원 
- 전체 Heap을 동일한 크기의 연속된 공간으로 나눔, 나뉘어진 각 영역들을 `region`이라고 함
- region들은 각각 `Eden`, `Survivor`, `Old`공간으로 논리적으로 매핑됨 (논리적 맵핑으로 쉽게 리사이징 가능)
-  그 이외의 region
	- `Humongous region` : 객체가 region 크기에 50% 이상 차지할때, 인접 region에 함께 저장
	- `unused areas` : Heap에서 아직 사용되지 않은 영역
- Minor GC
	- `STW`가 발생
	- Young 영역에서 Live Object 찾아서 적절한 region에 복사한다. (Oracle 문서엔 대피시킨다고 표현함)
	- Multi Thread 로 실행됨
- Major GC (Concurrent Marking Cycle)
	1. Concurrent Marking
		- Multi Thread로 동작하며, 애플리케이션과 Concurrnet하게 수행
		- Old영역에서 Live Object를 마킹함
		- Evacuation pause와 동시에 실행 가능하다 (=[Minor GC를 뜻함](http://initproc.tistory.com/archive/20130814))
	2. Remark (`STW`)
		- SATB 알고리즘 사용 (CMS보다 훨씬 빠르다고함) 
		- 완전히 비어있는 region이 반환됨
	3. Copying/Cleanup (`STW`)
		- Multi Thread로 동작
		- region에 liveness 정보를 갱신
> 이것만 봐서는 g1이 내부적으로 어떻게 동작하는지 도저히 와닿지가 않아, 검색을 해보니 설명이 굉장히 잘 나온 블로그가 있어서 별도로 기재한다. 
> [이곳](http://initproc.tistory.com/archive/20130814)에서 G1 GC의 수행단계를 확인하고,
> [이곳](http://appchemist.net/2013/04/27/garbage-first-garbage-collector/)에선 G1 GC internal 자료구조와 동작방식을 확인하면 좋을것 같다. 

---
#### 참고문헌
- [Introduction of Java GC Tuning and Java Java Mission Control](https://www.slideshare.net/leonjchen/java-optimization-twjug)
- [NAVER helloworld blog](http://d2.naver.com/helloworld/1329)
- [Oracle Getting Stared with the G1 Garbage Collector](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
- [initproc blog](http://initproc.tistory.com/archive/20130814)
- [appchemist blog](http://appchemist.net/category/programming/java/)







