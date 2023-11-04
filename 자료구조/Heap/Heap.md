# Heap

**힙**은 주로 **이진 트리(binary tree) 기반**으로 구현

- **트리(tree)**란 → 부모-자녀처럼 **계층적인 형태**를 가지는 구조

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled.png)

---

**힙**은 **max heap**과 **min heap**이 있음

**max heap**

- 부모 노드의 키(key)가 자식 노드(들)의 키(key)보다 크거나 같은 트리

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%201.png)

**min heap**

- 부모 노드의 키(key)가 자식 노드(들)의 키(key)보다 작거나 같은 트리
- min heap은 반대로생각하시면 됩니다.

---

**힙 주요 동작**

- insert → 힙에 아이템을 키값과 함께 넣어준다.(max heap 경우의 그림 예)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%202.png)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%203.png)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%204.png)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%205.png)

- delete → max heap의 경우 키값이 가장 큰 아이템을 꺼낸다.

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%206.png)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%207.png)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%208.png)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%209.png)

![Untitled](Heap%20c0e59e9919eb4143a83e5c44809215a5/Untitled%2010.png)

- peek → delete와 같지만 실제로 heap에서 삭제하진 않는다.

---

**Priority queue와 Heap의 관계**

힙(heap)의 키(key)를 우선순위(priority)로 사용한다면 힙은 우선순위 큐(priority queue)의 구현체가 된다.

우선순위 큐에는 힙 말고도 여러 구현체가 있을 수 있지만 힙이 효율이 제일 좋아서 힙을 대부분 많이 사용한다.

따라서 priority queue = heap은 아니다.

**priority queue**는 **ADT**이고 **heap**은 **data** **structure** 이다.

---

**우선순위 큐와 힙의 사용 사례**

- 프로세스 스케줄링(process scheduling)
- heap sort(힙 정렬) → n개의 item을 힙에다 넣은 다음에 차례차례 delete하면 정렬되어 나온다.

---

**힙(heap)메모리도 혹시?**

힙(heap)메모리의 힙은 오늘 공부한 힙과는 관련이 없다.

heap의 사전적인 의미가 범위라는 뜻이라기 때문에 힙 메모리의 힙은 메모리에 쌓여있는 더미라고 이해하자.