---
title: "Java Garbage Collection (1)"
date: 2018-01-07
categories:
 - Develop
 - JAVA
tags:
 - GC
---

#### GC가 하는일
- Heap 내에서 사용되지 않는 Object를 찾는다
- 해당 메모리를 회수한다

#### 들어가기 전에... 용어정리
- STW(Stop The World) : JVM이 GC를 실행하는 Thread를 제외한 나머지 모든 Thread를 멈추는것
- Reachable : 어떤 객체 유효한 참조가 있을때
- Unreachable : 어떤 객체에 유효한 참조가 없을때
- Root set : 객체의 참조여부를 파악하기 위한 최초의 유효한 참조
![Reachable/Unreachable 객체](http://d2.naver.com/content/images/2015/06/helloworld-329631-2.png)


#### GC 기본 프로세스
1. Step1. Marking
    - ![Marking](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide3.png)
	- GC가 Unreachable Object를 찾아낸다
	- 이때 모든 객체를 스캔하므로 시간이 오래걸린다
2. Step2. Normal Deletion
	- ![Normal Deletion](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide1b.png)
	- Unreachable Object를 삭제한다
	- 제거된 객체의 포인터를 남겨두었다가, 추후 새로운 객체가 할당되면 해당 위치에 추가한다
3. Step3.  Compacting
	- ![Compacting](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide4.png)
	- 성능을 위해 Reachable Object를 묶는다
	- Compacting을 과정을 수행함으로써, 삭제된 객체들 사이에 발생한 Fragmentation 문제와 재할당이 쉬워진다(마지막에만 객체를 추가하면됨)

---
#### 비효율적인 프로세스
![Object Life Time](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/ObjectLifetime.gif)
위에 명시한 GC의 기본 프로세스는 모든 객체를 스캔해야 하므로 비효율적이다. 위 표를 보면, 일반적인 애플리케이션에서 대부분의 객체의 LifeTime이 짧다. 때문에 Heap을 더 작은 파트(Young, Old, Permanent)로 나누어 퍼포먼스를 향상시킬 수 있다.

> Weak generational hypothesis
>  - 대부분의 객체는 금방 접근 불가능 상태가 된다(unreachable)
>  - 오래된 객체에서젊은 객체로의 참조는 아주 적게 존재한다

---
#### Oracle HotSpot Heap Structure
![HotSpot Heap Structure](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide5.png)
- Young 영역 (Young Generation)
- Old 영역 (Old Generation)
- Permanent 영역 (Permanent Generation)

---
##### HotSpot VM에서 더 빠른 메모리 할당을 위해 사용하는 방법
- TLAB (Thread Local Allocation Buffers)
	- 각 Thread에 `Eden`영역에 작은 부분을 미리 할당
	- 각 Thread들은 자신이 갖고있는 TLAB에만 접근할 수 있다. 
	- 활성화 옵션 -> `-XX:+UseTLAB`
- Bump the Pointer Allocation
	- `Eden`영역에 할당된 마지막 객체의 위치를 추적 (`Eden`의 맨위에 존재)
	- 새로 할당할 객체가 `Eden`영역에 넣을 수 있는 크기인지 판단
> Multi Thread 환경에서 Thread들이 `Eden`영역에 저장하려면 `Lock`이 발생해야하지만, TLAB을 통해 각자의 영역에 저장하므로, `Lock`없이 가능하다.

---
##### Young 영역
객체가 가장 먼저 생성되는 영역으로, `Eden`영역과 2개의 `Survivor`영역으로 구성되어있다. 해당 영역에서 이뤄지는 GC를 `Minor GC`라고 한다

###### Young 영역의 처리 순서
1. 새로 생성되는 대부분의 객체는 대부분 `Eden`에 생성
2.  `Eden`에서 GC가 발생 -> 살아남은 객체는 `Survivor` 영역중 한곳으로 이동
3. 동일한 `Survivor` 영역이 꽉 찰때까지 `2`번 반복
4. `Survivor`에서 GC 발생 -> 살아남은 객체는 다른 `Survivor`영역으로 이동, 이때 두개의 `Survivor`영역중 한곳은 `반드시` 비어있어야 한다
5. 1~4 과정을 반복하다가, 지속적으로 살아남은 객체는 `Old` 영역으로 이동 (`promotion`)

---
##### Old 영역
위 `Young` 영역에서 지속적인 GC에도 살아남은 객체들이 모이는 영역으로, 이곳에서 이뤄지는 GC를 `Full GC`라고 한다. `Full GC`는 `Minor GC`에 비해 `STW`시간이 훨씬 길기 때문에, 성능에 큰 영향을 끼친다. 

###### Old 영역 GC 종류
- Serial GC
- Parallel GC / Parallel Old GC
- CMS GC (Concurrent Mark & Sweep GC)
- G1 GC (Garbage First GC)

---
##### Permanent 영역
Heap 메모리 영역중 하나로, 애플리케이션이 실행될때 클래스의 메타데이터를 저장하는 영역
 - Class의  Meta 정보(이름, 패키지 주소 등)
 - Method의 Meta 정보
 - 상수 string / static object 등
 - JVM 내부 객체 / 컴파일러 최적화 정보(JIT)
 - 등등..

무분별한 Static Object의 사용과, HotDeploy로 인한 Meta Data 증가로 `PermGen Space OOM`를 방지하고자, 해당 영역을 `java8`	부터 삭제하였다.
- Class / Method의 메타데이터 / JVM 내부 객체 및 컴파일러 최적화 정보 -> MetaSpace로 이동
- 상수 string / static object -> Heap으로 이동

---
#### 참고문헌
- [NAVER helloworld blog](http://d2.naver.com/helloworld/329631)
- [NAVER helloworld blog](http://d2.naver.com/helloworld/1329)
- [Oracle java garbage collection basics](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
- [삵 blog](http://sarc.io/index.php/java/794-tlab-thread-local-allocation-buffers-plab-promotion-local-allocation-buffers)