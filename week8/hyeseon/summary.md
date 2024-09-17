# **프로세스 주소 공간**

## 개념

각각의 프로세스가 실행될 때 사용할 수 있는 메모리의 논리적인 구조를 의미합니다. 운영체게가 각 프로세스마다 독립적으로 할당하며 프로세스는 자신만의 메모리 공간을 가지고 있다고 할 수 있습니다.

프로세스 주소 공간은 보통 다음과 같이 나뉩니다.

1. 코드 영역(프로그램 실행 코드)
2. 데이터 영역(전역 및 정적 변수)
3. 힙 영역(동적 메모리 할당)
4. 스택 영역(함수 호출과 지역 변수)

## 목적

1. **프로세스 격리**

   각각의 프로세스는 독립적인 주소 공간을 가짐으로써 한 프로세스가 다른 프로세스의 메모리에 접근하거나 침해하는 것을 방지합니다. 이를 통해 시스템 안정성과 보안을 유지할 수 있습니다.

2. **메모리 보호**

   각 프로세스는 자신에게 할당된 메모리 공간 외의 다른 메모리 영역에 접근할 수 없도록 하여 잘못된 메모리 접근으로 인한 오류를 방지합니다.

3. **효율적인 메모리 관리**

   운영체제는 각 프로세스의 주소 공간을 독립적으로 관리하면서도 물리 메모리를 효율적으로 배분하고 가상 메모리 기법을 통해 실제 물리 메모리보다 더 큰 메모리를 사용할 수 있게 합니다.

4. **가상 메모리 활용**

   프로세스마다 가상 주소 공간을 제공하여 프로세스는 물리 메모리의 실제 크기와 상관없이 일관된 주소 공간을 사용할 수 있습니다. 이로 인해 메모리 자원의 효율적인 분배가 가능해집니다.


## 가상 메모리

### 개념

컴퓨터 시스템에서 RAM(물리 메모리)의 크기와 상관없이 더 많은 메모리를 사용할 수 있도록 운영체제가 제공하는 메모리 관리 기법입니다. 각각의 프로세스는 독립적인 **가상 주소 공간**을 가지고 있으며 실제 물리 메모리와 가상 메모리 간의 매핑을 통해 프로그램이 물리 메모리보다 큰 메모리 공간을 사용하는 것처럼 동작하게 합니다.

### 동작 방식

1. **가상 주소와 물리 주소**

   가상 메모리 시스템에서는 프로세스가 사용하는 가상 주소가 실제 메모리의 물리 주소와 다릅니다. CPU는 가상 주소를 통해 메모리에 접근하고 이 주소는 **메모리 관리 장치(MMU, 하드웨어 장치)**에 의해 물리 주소로 변환됩니다.

2. **페이지와 페이지 테이블**

   가상 메모리는 보통 일정 크기의 페이지 단위(일반적으로 4KB 크기)로 나뉘어 관리됩니다. 이 페이지들은 페이지 테이블을 통해 물리 메모리의 어느 부분에 저장되어 있는지 매핑됩니다. 각 프로세스는 자신만의 페이지 테이블을 가지며 운영체제는 이를 통해 가상 주소를 물리 주소로 변환합니다.

   > **페이지 테이블의 저장 위치**: 운영체제에 의해 물리 메모리(RAM)에 저장됩니다.

3. **페이지 폴트(Page Fault)**

   프로세스가 접근하려는 페이지가 물리 메모리에 없을 때 페이지 폴트가 일어납니다. 이때 운영체제는 해당 페이지를 디스크에서 읽어 물리 메모리에 로드하고 페이지 테이블을 갱신하여 다시 프로세스를 실행합니다.

4. **스왑**(swap)

   물리 메모리가 부족하면 운영체제는 사용하지 않는 메모리 페이지를 하드 디스크의 스왑 공간으로 이동시킵니다. 이를 swap out이라 합니다. 나중에 해당 페이지가 다시 필요하면 이를 swap in 하여 물리 메모리로 가져옵니다.

<details> 
<summary><h3>초기화하지 않은 변수가 저장되는 곳</h3></summary>
<div markdown="1">

BSS(Block Started By Symbol) 영역에 저장됩니다. 프로그램이 실행됙 전까지 값이 지정되지 않은 전역 변수나 정적 변수가 할당되는 메모리 영역입니다.

실행 시점에 운영체제에 의해 자동으로 0으로 초기화됩니다.

### 프로세스 주소 공간의 구성

1. 코드 영역

   실행할 프로그램의 코드(명령어)가 저장되는 곳

2. 데이터 영역

   초기화된 전역 변수와 정적 변수가 저장되는 곳

3. BSS 영역

   초기화되지 않은 전역 변수와 정적 변수가 저장되는 곳

4. 힙 영역

   동적 메모리 할당(ex. `new` 로 할당된 메모리)이 이루어지는 곳

5. 스택 영역

   함수 호출 시 사용되는 지역 변수, 매개 변수, 리턴 주소 등이 저장되는 곳

</div>
</details>

<details> 
<summary><h3>Stack과 Heap의 크기와 크기가 결정되는 순간</h3></summary>
<div markdown="1">
- 개발자가 아닌 사용자가 이 크기를 결정할 수 있는지

스택은 고정된 크기를 가지며 프로그램 실행 시 운영체제에 의해 결정됩니다. 스택은 함수 호출과 관련된 데이터를 저장하고 함수가 종료되면 자동으로 메모리를 해제합니다.

스택 크기는 일반적으로 운영체제가 결정하지만 사용자 또한 `ulimit` 명령어를 사용하여 프로세스의 스택 크기를 설정할 수 있습니다.

힙은 동적으로 크기가 결정되며 프로그램이 실행되는 동안 필요에 따라 운영체제가 메모리를 할당합니다. 힙은 동적 메모리 할당을 통해 객체나 배열과 같은 데이터를 저장하며 메모리 관리는 개발자가 직접해야 합니다.

힙 크기는 동적 메모리 할당을 통해 결정되지만 사용자도 특정 프로그램의 실행 옵션을 통해 힙 크기를 조정할 수 있습니다. 특히 JVM 환경에서 사용자가 힙의 초기 크가와 최대 크기를 설정할 수 있습니다.

```java
java -Xms[초기 힙 크기] -Xmx[최대 힙 크기] MyApp
```

</div>
</details>

<details> 
<summary><h3>Stack과 Heap 중 접근이 더 빠른 곳</h3></summary>
<div markdown="1">

스택이 더 빠릅니다.

고정된 크기의 데이터(지역 변수, 함수 매개변수 등)은 스택에 직접 저장됩니다. 따라서 스택 영역에 가면 바로 데이터에 접근 가능합니다.

반면, 동적으로 할당된 데이터는 힙에 저장되며 그에 대한 참조(포인터)가 스택이나 다른 메모리 공간에서 관리됩니다. 따라서 힙 영역에 접근하기 위해서는 포인터를 통해 접근해야 하므로 스택보다 속도가 더 늦습니다.

</div>
</details>

<details> 
<summary><h3>공간을 분할하는 이유</h3></summary>
<div markdown="1">

메모리 사용의 효율성 때문입니다.

프로그램에서 사용하는 데이터와 메모리의 종류에 따라 접근 패턴과 사용 주기가 다릅니다. 이를 효율적으로 관리하기 위해 메모리 영역을 나누어 사용하는 것이 효과적입니다.

코드 영역은 프로그램이 시작할 때 한 번만 로드되며 변경되지 않지만 데이터 영역은 계속해서 변경될 수 있습니다. 또한 스택과 힙은 동적으로 크기가 변화하므로 이를 분리하여 관리하는 것이 효율적입니다.

</div>
</details>

<details> 
<summary><h3>스레드의 주소 공간</h3></summary>
<div markdown="1">

스레드는 동일한 프로세스 내에서 실행되기 때문에 프로세스의 메모리 주소 공간을 공유합니다. 즉, 여러 스레드가 하나의 프로세스 내에서 같은 **힙 공간, 데이터 영역 및 코드 영역**을 공유합니다. 반면, 스레드마다 독립적입 스택 영역이 할당됩니다.

공유 데이터 영역으로 인해 스레드는 병렬 처리를 효율적으로 수행할 수 있지만 동기화 문제를 고려해야 합니다.

</div>
</details>

<details> 
<summary><h3>"스택"영역과 "힙"영역은 정말 자료구조의 스택/힙과 연관?</h3></summary>
<div markdown="1">

연관 없습니다.

</div>
</details>

<details> 
<summary><h3>IPC의 Shared Memory 기법은 프로세스 주소 공간 중 들어가는 곳</h3></summary>
<div markdown="1">

각 프로세스의 가상 주소 공간 내에 독립된 영역으로 매핑됩니다. 운영체제가 관리하는 물리 메모리의 특정 부분을 프로세스의 가상 주소 공간에 연결하여 사용합니다.

</div>
</details>


# **스케쥴러**

## 개념

스케줄러는 운영체제의 핵심적인 부분으로 **CPU가 여러 프로세스나 스레드 간에 자원을 효율적으로 할당**하는 역할을 합니다. 컴퓨터 시스템은 동시에 여러 작업을 처리해야 할 때가 많기 때문에 운영체제는 스케줄러를 통해 CPU를 각 작업에 적절히 배분하여 실행 시간을 결정합니다.

## 목적

1. 공정성

   여러 프로세스가 CPU 자원을 공정하게 나누어 사용할 수 있도록 보장합니다.

2. 효율성

   CPU의 사용률을 극대화하여 유휴 시간을 최소화합니다.

3. 응답 시간

   스케줄러는 시스템의 응답성을 유지하는 역할을 합니다.

4. 처리량

   주어진 시간 내에 더 많은 작업을 처리하는 것을 목표로 합니다.

5. 대기 시간과 응답 시간 최소화

   프로세스가 CPU를 받기 까지의 대시 시간을 줄이고 가능한 한 빠르게 응답이 이루어지도록 합니다.

6. 우선 순위 관리

   스케줄러는 프로세스의 우선 순위를 고려하여 중요한 작업이 먼저 실행되도록 합니다.


## 동작 방식

스케줄러는 단기, 중기, 장기 스케줄러로 나뉩니다.

### 단기 스케줄러

준비 상태에 있는 프로세스 중에서 다음에 실행될 프로세스를 선택하여 CPU에 할당하는 역할을 합니다.

### 중기 스케줄러

메모리 관리를 최적화하기 위해 실행 중인 프로세스를 일시적으로 메모리에서 디스크로 옮기고 나중에 다시 메모리로 불러옵니다.

### 장기 스케줄러

입출력 작업 대기 중인 프로세스나 새로운 프로세스가 시스템에 들어올 때 이들을 제어하는 역할을 합니다.

시스템에 들어오는 프로세스의 비율을 조절하여 시스템 전체의 작업량을 관리합니다.

## 스케줄링 알고리즘

스케줄러는 CPU를 효율적으로 할당하기 위해 여러 가지 스케줄링 알고리즘을 사용합니다. 대표적인 스케줄링 알고리즘은 다음과 같습니다.

### FCFS(First-Come, First-Served)

비선점형 스케줄링 방식으로, 먼저 도착한 프로세스가 먼저 실행되는 방식입니다. 간단하지만 긴 작업이 먼저 도착하면 짧은 작업들이 오랫동안 기다려야 하는 Convoy Effect 문제가 발생할 수 있습니다.

### SJF(Shortest Job First)

비선점형 스케줄링 방식으로, 가장 짧은 실행 시간을 가진 프로세스가 먼저 실행됩니다.

대기 시간을 최소화할 수 있지만 실행 시간이 긴 작업이 무한정 대기할 수 있는 기아(Starvation) 문제가 생길 수 있습니다.

### Round Robin(RR)

선점형 스케줄링 방식으로, 각 프로세스에 일정한 시간 할당량을 주고 그 시간이 지나면 다음 프로세스로 넘어가는 방식입니다.

주로 시분할 시스템에서 사용되며 각 프로세스에 공평한 CPU 시간을 부여합니다.

### Priority Scheduling

선점형 스케줄링 방식으로, 각 프로세스에 우선순위를 부여하고 우선순위가 높은 프로세스가 먼저 실행됩니다.

우선순위가 낮은 프로세스가 기아 상태에 빠질 수 있으며 이를 해결하기 위해 에이징(Aging) 기법이 사용됩니다.

### 다단계 큐 스케줄링

프로세스들을 여러 큐로 나누고 각 큐에 대해 다른 스케줄링 알고리즘을 사용합니다. 예를 들어, 상위 큐는 짧은 작업을 하위 큐는 긴 작업을 처리할 수 있습니다.

<details> 
<summary><h3>단기, 중기, 장기 스케쥴러와 현재 사용하는 스케쥴러</h3></summary>
<div markdown="1">

장기 스케줄러는 프로세스가 시스템에 들어오는 속도를 제어하는 역할을 합니다. 어떤 프로세스가 메모리에 적재되어 실행 대기 상태로 들어갈지 결정합니다.

중기 스케줄러는 실행 중인 프로세스를 메모리에서 제거하고(swap out), 나중에 다시 메모리로 불러오는(swap in) 역할을 합니다.

단기 스케줄러는 준비 상태(Ready Queue)에 있는 프로세스 중에서 어느 프로세스를 CPU에 할당할지 결정합니다. CPU 스케줄러라고도 불리며 매우 짧은 시간 단위로 동작합니다.

현대 운영체제는 주로 단기 스케줄러를 사용합니다. 리눅스의 스케줄러는 CFS(Completely Fair Scheduler)를 사용하며 각 프로세스가 공정하게 CPU 시간을 나눠받도록 설계되어 있습니다.

윈도우의 스케줄러는 우선순위 기반 스케줄링을 사용합니다. 우선순위가 같은 프로세스 간에는 라운드 로빈 방식으로 스케줄링이 이루어집니다.

macOS의 스케줄러는 우선순위 기반 선점형 스케줄러를 사용합니다. 각 프로세스에 우선순위를 부여하고 높은 우선순위 프로세스가 먼저 CPU를 사용할 수 있도록 합니다.

</div>
</details>

<details> 
<summary><h3>프로세스의 스케쥴링 상태</h3></summary>
<div markdown="1">

### **개념**

운영체제가 프로세스를 관리하는 과정에서 프로세스가 어떤 단계에 있는지를 나타내는 상태입니다.

### 상태
<img src="https://github.com/user-attachments/assets/fa021783-ad79-46c8-916a-898ff13caba5" width="50%"/>

1. **생성 상태 New**

   프로세스가 처음 생성된 상태입니다. 운영체제가 해당 프로세스를 메모리나 다른 시스템 자원에 등록하고 준비시킵니다. 이 상태에서 프로세스는 아직 실행되지 않았으며 운영체제는 프로세스를 준비 상태로 넘기기 위한 초기화를 수행 중입니다.

2. **준비 상태 Ready**

   프로세스가 실행을 위해 CPU를 기다리는 상태입니다.

   준비 큐(Ready Queue) : 준비 상태에 있는 프로세스들은 모두 준비 큐에 저장되며 스케줄러는 이 큐에서 프로세스를 선택하여 실행시킵니다.

3. **실행 상태(Running)**

   프로세스가 CPU를 할당받아 실제로 실행 중인 상태입니다.

   문맥 교환(Context Switch) : 실행 중이던 프로세스가 대기 상태로 전환되거나 더 높은 우선순위의 프로세스가 있으면 CPU가 다른 프로세스에게 넘어갈 수 있으며 이 과정에서 문맥 교환이 일어납니다.

4. **대기 상태(Waiting) 또는 블록 상태(Blocked)**

   대기 상태는 프로세스가 입출력 작업이나 특정 이벤트를 기다리며 CPU를 사용하지 않는 상태입니다. 요청한 작업이 완료되면 다시 준비상태로 돌아가 CPU 할당을 기다립니다.

5. **종료 상태(Terminated)**

   프로세스가 모든 작업을 완료하고 종료된 상태를 의미합니다. 운영체제는 해당 프로세스를 메모리에서 제거하고 자원을 반환합니다.

</div>
</details>

<details> 
<summary><h3>preemptive/non-preemptive 에서 존재할 수 없는 상태</h3></summary>
<div markdown="1">

비선점형 스케줄링에서는 한 번 CPU를 할당받은 프로세스는 스스로 CPU를 반납할 때까지 계속 실행됩니다. 따라서 존재할 수 없는 상태 전이는 실행 상태(Running) → 준비상태(Ready)입니다.

선점형 스케줄링에서는 운영체제가 언제든지 실행 중인 프로세스에서 CPU를 빼앗아 다른 프로세스에 할당할 수 있습니다. 이로 인해 모든 상태 전이가 가능합니다.

</div>
</details>

<details> 
<summary><h3>Memory가 부족할 경우, Process의 상태 변화</h3></summary>
<div markdown="1">

메모리가 부족할 때 운영체제는 중기 스케줄러를 통해 프로세스를 일시적으로 중단하고 swap 기법을 사용하여 메모리 자원을 관리합니다.

1. **준비 상태(Ready) → 대기 상태(Swapped Out)**

   메모리 부족이 발생하면 운영체제는 메모리에 있는 프로세스 중 실행되지 않는 프로세스를 디스크의 스왑 영역으로 옮겨 메모리 공간을 확보합니다(swap out)

2. **대기 상태(Swapped Out) → 준비 상태(Ready)**

   메모리 자원이 충분히 확보되면 운영체제는 스왑 아웃된 프로세스를 다시 메모리로 불러옵니다(Swap IN)

3. **실행 상태(Running) → 대기 상태(Waiting)**

   프로세스가 실행 중에 메모리 부족을 겪거나 프로세스가 추가적인 메모리 자원을 요청할 경우 입출력 작업을 기다리기 위해 대기 상태로 전환될 수 있습니다.

4. **실행 상태(Running) → 스왑 대기 상태(Swapped Out)**

   실행 중인 프로세스가 메모리 부족으로 인해 스왑 아웃될 수 있습니다. 운영체제는 현재 실행 중인 프로세스가 우선순위가 낮거나 메모리를 많이 사용하는 프로세스일 경우 이를 스왑 아웃해 메모리를 확보합니다.

5. **대기 상태(Waiting) → 준비 상태(Ready)**

   대기 상태에 있던 프로세스는 요청했던 메모리가 확보되거나 입출력 작업이 완료되면 다시 준비 상태로 돌아옵니다.

6. **종료 상태(Terminated)**

   메모리 부족으로 인해 시스템 안정성이 위협받을 경우 운영체제는 우선순위가 낮거나 메모리를 많이 사용하는 프로세스를 종료하여 메모리를 확보합니다.


</div>
</details>

# **컨텍스트 스위칭**

## 개념

운영체제가 현재 실행 중인 프로세스 또는 스레드의 상태(Context)를 저장하고 다른 프로세스 또는 스레드의 상태로 전환하는 과정을 말합니다.

> **프로세스의 상태(Context)** : 프로세서 레지스터 값, 프로그램 카운터, 스택 포인터, 메모리 관리 정보 등이 포함됩니다.
>

## 목적

**멀티태스킹과 멀티프로세싱 환경**에서 여러 프로세스나 스레드가 CPU를 공정하게 사용할 수 있도록 하는 것입니다. 이를 통해 CPU 자원을 효율적으로 관리하고 시스템의 응답성을 높일 수 있습니다.

## 동작 방식

1. 실행 중인 프로세스가 CPU 사용을 마치거나 강제로 중단될 경우 운영체제는 해당 프로세스의 레지스터 값, 프로그램 카운터, 스택 포인터 등의 정보를 PCB에 저장합니다.
2. 스케줄러가 다음으로 실행할 프로세스를 선택합니다.
3. 선택된 프로세스의 이전 상태를 복원하여 해당 프로세스를 CPU에서 실행할 수 있도록 설정합니다.
4. CPU 제어가 새로운 프로세스에 넘어가면서 해당 프로세스는 자신의 작업을 계속 진행합니다.

## 컨텍스트 스위칭이 발생하는 상황

1. **선점형 스케줄링(Preemptive Scheduling)**

   현재 실행 중인 프로세스가 일정 시간(타임 슬라이스)을 사용한 후, **다른 프로세스에 CPU를 넘겨줄 때** 컨텍스트 스위칭이 발생합니다.

2. **입출력 대기(I/O Wait)**

   프로세스가 **입출력 작업을 기다리는 동안**, CPU가 유휴 상태에 머물지 않도록 다른 프로세스에게 CPU를 할당하는 과정에서 컨텍스트 스위칭이 발생합니다.

3. **우선순위 변경**

   더 높은 우선순위의 프로세스가 도착하면, 현재 실행 중인 프로세스가 **CPU를 강제로 빼앗기고 준비 상태로 전환**되면서 컨텍스트 스위칭이 일어납니다.


<details> 
<summary><h3>컨텍스트 스위칭이 발생했을 때 프로세스와 스레드의 차이</h3></summary>
<div markdown="1">

| 항목 | **프로세스 컨텍스트 스위칭** | **스레드 컨텍스트 스위칭** |
| --- | --- | --- |
| **주소 공간 전환** | 필요 (프로세스 간 독립된 메모리 공간) | 불필요 (같은 프로세스 내 스레드는 주소 공간을 공유) |
| **저장 및 복원할 정보** | 전체 메모리 관리 정보, 페이지 테이블, 레지스터, 프로그램 카운터 등 | 레지스터, 프로그램 카운터, 스택 포인터 등 |
| **오버헤드** | 크다 (주소 공간 전환 필요) | 작다 (주소 공간 공유로 전환 불필요) |
| **속도** | 상대적으로 느림 | 상대적으로 빠름 |
| **데이터 공유** | 프로세스 간 데이터 공유 어려움 | 스레드 간 데이터 공유 용이 (동일 메모리 공간 사용) |

</div>
</details>

<details> 
<summary><h3>컨텍스트 스위칭이 발생할 때, 기존의 프로세스 정보가 커널스택에 저장되는 형식</h3></summary>
<div markdown="1">

### 커널 스택

각 프로세스가 커널 모드에서 실행될 때 사용하는 스택 메모리 공간입니다. 운영체제는 모든 프로세스마다 고유한 커널 스택을 하나씩 할당하며 각 커널 스택은 해당 프로세스의 커널 모드 상태에서의 정보(레지스터 값, 시스템 콜 정보 등)를 저장하고 관리합니다.

컨텍스트 스위칭이 발생할 때 현재 실행 중인 프로세스의 상태를 저장해야 합니다. 이 상태 정보가 커널 스택에 저장됩니다. 커널 스택에는 다음과 같은 것들이 저장됩니다.

1. CPU 레지스터 값
2. 프로그램 카운터
3. 스택 포인터
4. 플래그 레지스터(Flags Register)

</div>
</details>

<details> 
<summary><h3>컨텍스트 스위칭이 발생하는 시점</h3></summary>
<div markdown="1">

위 내용 참고

</div>
</details>

# **프로세스 스케쥴링 알고리즘**

### FCFS(First-Come First-Served, 선입 선출)

가장 먼저 도착한 프로세스가 먼저 CPU를 할당 받는 방식입니다. 비선점형 방식이며 구현이 간단합니다. 다만 Convoy Effect가 발생할 수 있습니다.

> **Convoy Effect**: CPU를 많이 필요로 하지 않는 프로세스들이 CPU를 오랫동안 사용하는 프로세스가 끝나기를 기다리는 현상
>

### SJF(Shortest Job First, 최단 작업 우선)

실행 시간이 가장 짧은 작업을 먼저 처리하는 방식입니다. 비선점형 방식이며 최적의 평균 대기 시간을 제공하는 알고리즘 중 하나입니다. 다만 프로세스의 실행 시간을 미리 알기 어렵기 때문에 실제로 적용하기 어려우며 기아 현상(Starvation)이 발생할 수 있습니다.

> **Starvation**: 실행 시간이 긴 프로세스가 계속 대기하는 현상
>

### Round Robin

각 프로세스에 동일한 시간 할당량을 주고 그 시간만큼 실행한 후 다음 프로세스로 넘어가는 방식입니다. 모든 프로세스가 순차적으로 CPU를 할당받아 공정하게 실행됩니다. 선점형 스케줄링 방식이며 시스템 응답성이 중요한 경우에 유용합니다.

### Priority Scheduling

우선순위가 높은 프로세스에 CPU를 먼저 할당하는 방식입니다. 각 프로세스는 우선순위 값을 가지고 있으며 이 값에 따라 CPU를 할당 받습니다. 선점형 또는 비선점형으로 동작할 수 있습니다.

기아 현상이 발생할 수 있으며 이를 해결하기 위해 에이징(Aging) 기법이 사용되기도 합니다.

> **에이징(Aging)** : 시간이 지날수록 우선순위를 높여주는 방식
>

### Multilevel Queue Scheduling

프로세스들을 여러 우선순위 큐로 나누어 각각 다른 스케줄링 알고리즘을 적용하는 방식입니다. 큐 간 우선순위가 정해져 있으며 상위 큐가 비어있지 않으면 하위 큐의 프로세스는 실행되지 않습니다.

### Multilevel Feedback Queue Scheduling

다단계 큐 스케줄링의 한 형태로 프로세스가 자신의 실행 성능에 따라 다른 큐로 이동할 수 있는 방식입니다. CPU를 오래 사용한 프로세스는 하위 큐로 이동하고 짧은 작업은 상위 큐에서 처리됩니다.

동적인 우선순위 조정이 가능하며 기아 현상을 방지할 수 있습니다. 다만 구현이 복잡합니다.


<details> 
<summary><h3>RR을 사용할 때, Time Slice에 따른 trade-off</h3></summary>
<div markdown="1">

Time Slice가 너무 길면 FCFS와 비슷해지고 너무 짧으면 Context Switching이 자주 발생해 오버헤드가 커집니다.

</div>
</details>

<details> 
<summary><h3>싱글 스레드 CPU 에서 상시로 돌아가야 하는 프로세스가 있을 때 사용하는 스케쥴링 알고리즘</h3></summary>
<div markdown="1">

우선순위 기반 스케줄링을 사용합니다.

상시로 돌아가야 하는 중요한 프로세스에 가장 높은 우선순위를 부여하여 다른 프로세스보다 항상 먼저 CPU를 사용할 수 있게 설정하는 방식입니다.

</div>
</details>

<details> 
<summary><h3>동시성 vs 병렬성</h3></summary>
<div markdown="1">

### 동시성(Concurrency)

여러 작업이 논리적으로 동시에 실행되는 것처럼 보이도록 관리하는 방식입니다. 실제로는 하나의 CPU에서 여러 작업이 짧은 시간 단위로 빠르게 교체되며 실행됩니다. 싱글 코어에서도 구현 가능하며 프로세스나 스레드 간의 빠른 Context Switching을 통해 이루어집니다. 실제로 동시에 실행되는 것은 아니기 때문에 대규모 작업 처리에 한계가 있습니다.

### 병렬성(Parallelism)

여러 작업을 물리적으로 동시에 처리하는 방식입니다. 주로 멀티코어 또는 멀티프로세서 시스템에서 이루어지며 각 코어가 서로 다른 작업을 동시에 처리할 수 있습니다. 각 코어는 별도의 작업을 물리적으로 동시에 처리하므로 진정한 의미에서의 동시 실행이 가능합니다.

</div>
</details>

<details> 
<summary><h3>타 스케쥴러와 비교하여, Multi-level Feedback Queue가 해결하는 문제점</h3></summary>
<div markdown="1">

1. **기아(Starvation) 문제**

   우선순위가 낮은 프로세스가 오랫동안 CPU를 할당받지 못하는 문제를 **동적 우선순위 조정**을 통해 해결하여, 모든 프로세스가 결국 CPU를 사용할 수 있게 합니다.

2. **고정된 우선순위 문제**

   다단계 큐 스케줄링에서 프로세스가 한 번 큐에 배치되면 다른 큐로 이동할 수 없다는 문제를 해결하기 위해 **큐 간의 이동**을 허용하여 **작업 성격에 따라 적절히 우선순위를 조정**할 수 있게 합니다.

3. **작업 성격에 따른 처리 효율성**

   짧은 작업은 상위 큐에서 빠르게 처리하고, 긴 작업은 하위 큐에서 더 긴 시간 할당량으로 처리하여 **작업에 따른 효율적인 스케줄링**을 가능하게 합니다.

4. **문맥 교환 오버헤드 문제**

   짧은 작업에 대해서는 짧은 시간 할당량을 적용하고, 긴 작업에 대해서는 더 긴 시간 할당량을 적용하여 **문맥 교환 오버헤드를 최소화**합니다.

</div>
</details>

<details> 
<summary><h3>스레드의 스케줄링 알고리즘</h3></summary>
<div markdown="1">

프로세스 스케줄링 알고리즘과 유사한 원리로 작동하지만 스케줄링 단위가 스레드입니다.

</div>
</details>

# **가상화가 무엇이고, 이것이 가상머신과 어떠한 차이가 있는지 설명해 주세요.**

## 개념

가상화는 하드웨어 자원을 추상화하여 여러 운영체제나 애플리케이션이 물리적 자원과 독립적으로 실행될 수 있도록 하는 기술입니다. 가상화는 물리적 하드웨어를 논리적으로 분리해 하나의 물리적 시스템에서 여러 개의 가상 시스템을 구동할 수 있게 합니다. 이 기술을 통해 서버, 스토리지, 네트워크 등의 하드웨어 자원을 소프트웨어적으로 나눠 사용할 수 있으며 여러 작업을 서로 독립적인 환경에서 실행할 수 있습니다.

## 목적

1. **자원의 효율적 사용**

   하나의 물리적 하드웨어에서 여러 개의 가상 환경을 만들어 자원을 효율적으로 활용할 수 있습니다.

2. **유연성 및 확장성**

   가상화된 환경에서는 필요에 따라 자원을 쉽게 추가하거나 감소시킬 수 있습니다.

3. **테스트 및 개발 환경 분리**

   여러 가상 환경을 생성하여 각각을 독립적인 테스트 및 개발 공간으로 사용할 수 있습니다.

4. **비용 절감**

   하드웨어 자원을 최적화함으로써 서버 비용을 절감할 수 있습니다.

5. **보안과 격리**

   가상화된 시스템은 각기 독립적으로 실행되기 때문에 한 가상 시스템에서 문제가 발생해도 다른 가상 시스템에 영향을 미치지 않습니다.


## 가상머신과의 차이

가상화는 물리적 하드웨어 자원을 효율적으로 활용하고 여러 환경을 분리하여 제공하는 기술적인 개념입니다. 가상머신은 가상화를 통해 만들어진 논리적인 독립적인 컴퓨터 환경으로 각각의 가상머신은 실제 하드웨어와 독립적으로 운영체제와 애플리케이션을 실행할 수 있습니다.

<details> 
<summary><h3>그렇다면 Docker는 둘 중 어디에 속하나요? 왜 사람들이 Docker를 많이 채택할까요?</h3></summary>
<div markdown="1">

Docker는 가상화 기술의 한 형태이지만 가상머신과 다르게 경량화된 가상화 기술, 즉 컨테이너 기반의 가상화로 분류됩니다.

Docker는 **가볍고 빠른 실행**이 가능하며, **이식성, 확장성, 보안성** 등의 장점을 제공합니다. 특히 **개발부터 배포까지**의 과정에서 일관된 환경을 제공하고 효율적인 자원 관리를 가능하게 하므로 많은 개발자와 기업에서 채택되고 있습니다.

가상머신과 달리 Docker는 **빠른 배포**와 **적은 자원 사용**이 특징이므로 **클라우드 환경**이나 **마이크로서비스 아키텍처**에서 특히 많은 인기를 끌고 있습니다.

</div>
</details>

<details> 
<summary><h3>하나의 Host OS에서 돌아간다면 충분히 한 컨테이너가 다른 컨테이너에 간섭할 수 있는 위험이 있지 않을까요? 이를 어떻게 방어할 수 있을까요?</h3></summary>
<div markdown="1">

네 맞습니다. Docker 컨테이너는 하나의 Host OS에서 동작하며 같은 커널을 공유하기 때문에 보안적인 측면에서 다른 컨테이너와 간섭할 위험이 존재할 수 있습니다. 하지만 Docker는 **Namespace, Cgroups, Capabilities, Seccomp, AppArmor, SELinux**와 같은 **보안 기능과 자원 제어 메커니즘**을 통해 컨테이너 간의 **격리**와 **보안**을 강화합니다. 이러한 기술을 적절히 활용하면 **컨테이너 간의 간섭**을 최소화하고 **안전한 멀티컨테이너 환경**을 유지할 수 있습니다.


</div>
</details>

<details> 
<summary><h3>Docker 위에 Docker를 올릴 순 없을까요?</h3></summary>
<div markdown="1">

가능합니다. 하나의 컨테이너 내부에서 또 다른 Docker 컨테이너를 실행할 수 ㅣㅆ습니다.

</div>
</details>

