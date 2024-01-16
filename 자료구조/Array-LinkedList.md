# ArrayList와 LinkedList

**배열**을 이용해 구연한 **ArrayList**와 **포인터**를 이용해 구연한 **LinkedList**에 대해 알아보자.

## 1. ArrayList에 대해

ArrayList의 논리적 순서는 메모리 공간에서 저장되는 ‘원소값의 물리적 순서’와 같다.

예를 들어 다음과 같은 ArrayList를 선언하고 초기화했다고 해보자.

```java
int[] nums = {1,10,100};
```

위 코드는 메모리상에

![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/852b69f9-0c74-4ade-b710-e812ea9fae95)

이렇게 물리적으로 연속된 형태로 저장될 것이다. 이걸 **순차 리스트**라고 한다.

따라서 ArrayList의 첫 번째 원소의 주소를 가지고 인덱스를 이용해 임의의 배열 원소의 주소값을 계산해 접근할 수 있다.

## 2. ArrayList의 특징

- 고정된 크기를 가지고 크기를 늘리거나 줄일 수 없다.
    - ArrayList의 크기를 6으로 선언했다면 ArrayList에 데이터가 3개만 들어가있어도 배열의 크기는 6이고 3만큼의 메모리가 낭비될 수 있다.
- 논리적 순서가 물리적 순서와 같다.
- Cache Hit Rate가 높다.
- 시간복잡도
    - 삽입, 삭제 : O(N)
    - 접근 :  O(1)

### 잠깐 왜 삽입, 삭제시에 시간복잡도가 O(N)이 나올까?

삽입, 삭제시 평균적으로 N/2 만큼 원소를 밀거나 당겨와야 해서 그렇다!

## 3. ArrayList의 삭제 연산

예를 들어 아래와 같은 배열이 있다고 해보자.

| 10 | 20 | 30 | 40 | 50 | 60 |
| --- | --- | --- | --- | --- | --- |

2번 인덱스에 있는 원소 30을 삭제합니다.

| 10 | 20 |  | 40 | 50 | 60 |
| --- | --- | --- | --- | --- | --- |

메모리에 연속되게 위치해야 하므로 4번 인덱스부터 1칸씩 당긴다.

| 10 | 20 | 40 | 50 | 60 |  |
| --- | --- | --- | --- | --- | --- |

따라서 자료의 삽입과 삭제가 빈번히 발생하는 상황에서 List를 Array로 구현하는 것은 빈번한 자료 이동으로 인하여 비효율을 유발하기 때문에 실제 IT 서비스 환경에서는 자주 사용되지 않고 있다.

---

## 1. LinkedList에 대해

LinkedLinst는 포인터를 이용해 구현된다. 각 노드는 원소값과 다음 원소의 주소값(포인터)를 저장하고있다.

<!--![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/bfd887a5-b1a0-4b53-8691-4a03843bd2b4)-->
<img src="https://github.com/limjoohyun2030/CS-study/assets/91045946/bfd887a5-b1a0-4b53-8691-4a03843bd2b4" width=50%>

![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/e06e511e-81d8-4935-84a6-8240e1e8aa4b)


## 2. LinkedList 의 특징

- ArrayList와 달리 메모리에 연속적으로 있어야 하는게 아니어서 이 때문에 메모리의 낭비가 발생하진 않는다.
- 논리적 순서와 물리적 순서가 다르다.
- 위 이유로 Cache Hit Rate가 낮다.
- 시간복잡도
    - 삽입, 삭제 : O(N)
    - 접근 : O(N)
    

## 3. LinkedList의 삭제, 삽입 연산

<!--![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/8cafe75f-ecd2-44d6-852a-ea9858676f5a)-->
<img src="https://github.com/limjoohyun2030/CS-study/assets/91045946/8cafe75f-ecd2-44d6-852a-ea9858676f5a" width="300" height="550">

### 삭제 연산

**노드 x를 삭제할 때 :**

![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/9fbf5f82-aae8-49dc-bd06-6dafc3b5ecef)


- 삭제할 노드의 선행 노드의 링크 필드를 삭제할 노드의 후행 노드를 가리키게 한다.
- 삭제할 노드를 메모리에 반환한다.
    
    ![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/4bf50e3d-53b9-471e-8727-d4c9f111dd4d)

    
    ### 삽입 연산
    
    노드 i 와 j 사이에 x 노드를 삽입할 때 :
    
    **연결 리스트의 초기 모습**
    
    ![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/b7c5c6c3-795f-4e8f-b94a-d3fc4f7a7651)

    
    **연결 리스트의 삽입 결과**
    
    ![image](https://github.com/limjoohyun2030/CS-study/assets/91045946/4210984f-1f09-4bcb-8c1d-6b309af9860a)

    
- i 노드의 j 노드를 향하는 링크를 끊는다.
- i 노드의 링크 부분을 x 노드를 향하게 한다.
- x 노드의 링크 부분을 j 노드를 향하게 한다.

- 참조
    - 강태원, 정광식. 2023. 자료구조. 2개정판 1쇄. 서울 : 한국방송통신대학교 출판문화원.
    - KOCW. 정광식. 자료구조([https://ucampus.knou.ac.kr/ekp/user/course/initUCRCourse.sdo?pageIndex=1&recordCountPerPage=4&sbjtId=KNOU2135001&cntsId=KNOU2135&atlcNo=10704052&tespNo=&lectPldcTocNo=&examApexNo=&burSbjtCd=&tabNo=01&dblMjSbjtYn=N&curSbjtId=&curLectPldcTocNo=&systemDiv=H&searchCntsCateNo=34&searchShgr=&searchSeme=](https://ucampus.knou.ac.kr/ekp/user/course/initUCRCourse.sdo?pageIndex=1&recordCountPerPage=4&sbjtId=KNOU2135001&cntsId=KNOU2135&atlcNo=10704052&tespNo=&lectPldcTocNo=&examApexNo=&burSbjtCd=&tabNo=01&dblMjSbjtYn=N&curSbjtId=&curLectPldcTocNo=&systemDiv=H&searchCntsCateNo=34&searchShgr=&searchSeme=))
    - 옹벨 일기. [자료구조] Array(배열) vs List(리스트)([https://ongveloper.tistory.com/403](https://ongveloper.tistory.com/403))
