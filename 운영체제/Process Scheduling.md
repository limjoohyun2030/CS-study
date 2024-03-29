## 스케줄링의 개념

다중 프로그래밍에서 **프로세서를 할당할 프로세스를 선택할 때는 어떤 전략**(정책)이 필요한데, 여기서  필요한 개념이 **스케줄링**이다. **스케줄링은** 여러 프로세스가 번갈아 사용하는 자원을 어떤 시전에 **어떤 프로세스에 할당할지 결정**하는 것이다.

1. 프로세서 이용률을 높일 수 있다.
2. 프로세서 처리율(주어진 시간에만 처리하는 작업량)이 증가한다.

**스케줄링이 필요 없는 프로세스**도 있는데, 인터럽트 처리, 오류 처리, 시스템 호출 등의 사전 처리가 대표적인 예이다.

## 스케줄링 단계

1. **장기 스케줄링 (Long-Term Scheduling):**
    - 장기 스케줄링은 **메모리에 적재되기 전의 프로세스들을 대상**으로 하는 스케줄링이다.
    - CPU에 적재할 프로세스 중에서 **어떤 프로세스를 선택하여 메모리에 적재할지 결정**한다.
    - 이 과정은 프로세스의 메모리 요구량, 실행 시간 등을 고려하여 실행할 프로세스들을 선별한다.
    - 장기 스케줄링은 시스템의 처리량과 사용자 응답 시간을 최적화하기 위해 필요하다.
    - 프로세스들을 **메모리에 적재하고 준비 큐로 이동시**킨다.

1. **중기 스케줄링 (Medium-Term Scheduling):**
    - 중기 스케줄링은 **메모리에 적재되어 실행 중인 프로세스들을 대상**으로 하는 스케줄링이다.
    - 메모리에 적재된 프로세스 중에서 **일시적으로 메모리를 해제하여 디스크로 이동시킬지 결정**한다.
    - 이는 메모리 부족 상황을 완화하거나 멀티프로그래밍 정도를 조절하는 데 사용된다.
    - 디스크로 이동된 프로세스는 나중에 다시 메모리에 적재되어 실행된다.
    - 중기 스케줄링은 **메모리 관리**를 효율적으로 하고, 프로세스의 메모리 요구량을 관리하는 데 사용된다.
2. **단기 스케줄링 (Short-Term Scheduling):**
    - 단기 스케줄링은 **현재 메모리에 적재되어 있는 프로세스들을 대상**으로 하는 스케줄링이다.
    - **CPU를 할당할 프로세스를 선택**하고, 실제로 CPU를 할당하는 과정을 수행한다.
    - 이 과정은 프로세스의 우선순위, 실행 시간 등을 고려하여 CPU를 할당할 프로세스를 결정한다.
    - 단기 스케줄링은 시분할 시스템에서 다중 프로그래밍 정도를 관리하고, 응답 시간을 단축하는 데 사용된다.

## 스케줄링 목표

1. **공정성(Fairness):**
    - 모든 프로세스들이 CPU를 공평하게 나눠쓰도록 하는 것을 목표로 한다.
    - 각 프로세스들이 **적절한 시간 동안 실행되어 자원을 공평하게 이용**할 수 있도록 스케줄링한다.
2. **처리량(Throughput):**
    - **주어진 시간 동안 시스템이 처리하는 프로세스의 수나 작업량을 최대화**하는 것을 목표로 한다.
    - 시스템이 가능한 한 많은 프로세스를 처리하여 자원을 효율적으로 활용한다.
3. **응답 시간(Response Time):**
    - 프로세스가 요청한 작업을 **실행을 시작하는데 걸리는 시간을 최소화**하는 것을 목표로 한다.
    - 사용자가 입력한 작업에 대한 빠른 응답을 제공하여 시스템의 반응성을 높이는 것이 중요하다.
4. **반환 시간(Turnaround Time):**
    - 프로세스가 시스템에 **도착한 후 작업이 완료될 때까지 걸리는 시간을 최소**화하는 것을 목표로 한다.
    - 프로세스들이 빠르게 처리되어 작업이 완료되는데 걸리는 시간을 최소화한다.
5. **자원 활용률(Utilization):**
    - 시스템의 자원, 특히 CPU를 최대한 활용하는 것을 목표로 한다.
    - 대기 시간을 최소화하고, CPU가 유휴 상태가 되는 것을 방지하여 자원을 효율적으로 사용한다.

### 용어 정의

| 기준 | 정의 |
| --- | --- |
| 처리량
(throughput) | 주어진 시간에 처리한 프로세스 수 |
| 반환시간
(turnaround time) | 프로세스 생성 시점부터 종료 시점까지의 소요시간 |
| 응답시간
(response time) | 요청한 시점부터 반응이 시작되는 시점까지의 소요시간 |
| 대기시간
(waiting time) | 프로세스가 종료될 때까지 준비 큐에서 기다린 시간의 합 |

## 스케줄링 정책

앞서 살펴본 **스케줄링의 목표들은 각각 서로 상반된 경우가 있어 모두를 한 번에 충족시키는 정책은 세우기가 어렵다**. 예를 들어, 처리량을 극대화하기위해 단위시간당 완료되는 프로세스 수가 많아지도록 수행시간이 짧은 프로세스를 우선 처리할 수 있지만 그런 경우 수행시간이 긴 프로세스는 대기시간과 반환이 길어지게 된다.

1. **선점 스케줄링 정책(preemptive scheduling policy)**
    1. 실행 중인 프로세스에 **인터럽트를 걸고 다른 프로세스에 CPU를 할당할 수 있는** 스케줄링 방식이다.
2. **비선점 스케줄링 정책(nonpreemptive scheduling policy)**
    1. CPU를 할당받아 실행이 시작된 프로세스는 대기상태나 종료상태로 전이될 때까지 **계속 실행상태**에 있게 된다.

## 스케줄링 평가 기준

일반적으로 **평균대기시간과 평균반환시간이 이용**된다.

![Untitled](https://raw.githubusercontent.com/limjoohyun2030/CS-study/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled.png)

1. **평균 대기 시간 계산:**
평균 대기 시간은 각 프로세스의 대기 시간을 합산하여 프로세스의 개수로 나누는 방식으로 계산.
대기 시간은 해당 프로세스가 실행 큐에서 실행되기까지 대기한 시간.
    
    평균 대기 시간 = (모든 프로세스의 대기 시간의 합) / (프로세스의 총 개수)
    
    예를 들어, 프로세스 A, B, C가 각각 대기 시간이 3, 5, 2라고 하면,
    평균 대기 시간 = (3 + 5 + 2) / 3 = 3.33 시간 (단위는 시간)
    
2. **평균 반환 시간 계산:**
평균 반환 시간은 각 프로세스의 반환 시간을 합산하여 프로세스의 개수로 나누는 방식으로 계산.
반환 시간은 해당 프로세스가 시스템에 도착한 후 작업이 완료될 때까지 걸린 시간.
    
    평균 반환 시간 = (모든 프로세스의 반환 시간의 합) / (프로세스의 총 개수)
    
    예를 들어, 프로세스 A, B, C가 각각 반환 시간이 10, 15, 8이라고 하면,
    평균 반환 시간 = (10 + 15 + 8) / 3 = 11 시간 (단위는 시간)
    

## 스케줄링 알고리즘

1. **FCFS(First-Come, First-Served) 스케줄링: 비선점**
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/raw/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled%201.png)
    
    - 장점:
        - 구현이 간단하고 이해하기 쉽다.
    - 단점:
        - 평균 대기 시간이 높을 수 있다. 먼저 도착한 프로세스가 긴 작업을 할 경우, 뒤에 있는 모든 프로세스들이 대기해야 한다.
2. **SJF(Shortest Job First) 스케줄링: 비선점**
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/raw/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled%202.png)
    
    - 장점:
        - 일괄처리 환경에서 구현하기 쉽다.
    - 단점:
        - 실제로는 실행 시간을 사전에 알 수 없어 예측이 어렵다.
3. **SRT(Shortest Remaining Time) 스케줄링: 선점** 
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/raw/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled%203.png)
    
    - 장점:
        - SJF보다 평균대기시간과 평균반환시간에서 효율적이다.
    - 단점:
        - • 각 프로세스의 실행시간을 추적해야 하고 선점을 위한 문맥교환이 일어나기 때문에 SJF보다 오버헤드가 크다.
4. **RR(Round Robin) 스케줄링: 선점** 
    - 장점:
        - 모든 프로세스들에게 CPU 시간을 공평하게 할당. 작은 양의 시간 단위로 각 프로세스들을 번갈아가며 실행.
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/raw/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled%204.png)
    
    - 단점:
        - 시간 할당량(Time Quantum) 크기에 따라 성능이 크게 달라질 수 있다. 시간 할당량이 크면 SJF와 유사한 성능을 보이고, 작으면 오버헤드가 증가할 수 있다.
        
        ![Untitled](https://github.com/limjoohyun2030/CS-study/raw/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled%205.png)
        
5. **HRN(Highest Response Ratio Next) 스케줄링: 비선점**

![Untitled](https://github.com/limjoohyun2030/CS-study/raw/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled%206.png)

- 장점:
    - 응답 비율을 고려하여 프로세스를 스케줄링. 응답 비율이 높은 프로세스를 우선적으로 실행.
    - SJF와 RR의 장점을 혼합하여 평균 반환 시간과 응답 시간을 개선.
- 단점:
    - 우선순위 계산에 복잡성이 있어 구현이 어렵고 오버헤드가 증가할 수 있다.
1. **다단계 피드백 큐(Multilevel Feedback Queue) 스케줄링: 선점**
    - 선점 스케줄링 알고리즘
    - I/O 중심 프로세스와 CPU 중심 프로세스 특성에 따라 서로 다른 시간 할당량을 부여한다.
    - n개의 단계를 가지고 각 단계마다 하나의 큐가 존재한다.
    - 단계가 커질수록 시간 할당량도 커진다.
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/raw/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Process%20Scheduling/Untitled%207.png)
    
    ### 스케줄링 방법
    
    - 새로운 프로세스는 단계 1의 큐에서 FIFO(First In First Out) 순서에 따라 CPU를 점유한다.
    - 입출력 이벤트가 발생하면 CPU를 양보하고 대기상태로 갔다가 다시 준비상태가 될때는 현재와 동일한 단계의 큐에 배치된다.
    - 시간 할당량을 다 쓰고 작업을 못마친 경우에는 다음 단계의 큐로 이동해 배치된다.
    - 마지막 단계에서는 RR 스케줄링 방식으로 동작한다.
    - 단계 k의 큐에 있는 프로세스가 CPU를 할당 받으려면 단계 1부터 단계 k-1까지 모든 큐가 비어있어야 한다.
    
    **장점**
    
    - I/O 위주의 프로세스는 높은 우선권을 유지한다.
    - 연산 위주의 CPU 중심 프로세스는 낮은 우선권에 긴 시간 할당량을 가진다.
    
    ### 적응적 다단계 피드백 큐 스케줄링
    
    - 시간 할당량을 다 쓰기 전에 CPU를 반납하면 한단계 전의 큐로 이동해 배치된다.
    - 연산 위주의 프로세스가 I/O 위주로 바뀌면 점점 작은 단계로 배치될 수 있다.
    
- 참조
    - 구현회. 2021. 그림으로 배우는 구조와 원리 운영체제. 개정 3판. 서울 : 한빛아카데미.
    - 김진욱, 이인복. 2023. 운영체제. 초판. 서울 : 한국방송통신대학교 출판문화원.
    - KOCW. 양희재. 운영체제([http://www.kocw.net/home/search/kemView.do?kemId=978503](http://www.kocw.net/home/search/kemView.do?kemId=978503))
    - inhalin. 3강 스케줄링 알고리즘([https://velog.io/@inhalin/knou-os-03](https://velog.io/@inhalin/knou-os-03))
