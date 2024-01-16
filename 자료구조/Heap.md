**힙**은 **우선순위 큐의 한 종류**이고 **우선순위 큐를 트리로 구현한게 힙**이다.

**힙**은 **완전 이진 트리**이다. 때문에 **배열**로 구현해도 **메모리의 낭비가 없다** 따라서 배열로 구현한다.

- **트리tree**란 → 부모-자녀처럼 **계층적인 형태**를 가지는 구조

![](https://velog.velcdn.com/images/leequiett/post/d438e701-06a2-430d-b83b-8b992eaac1ae/image.png)

---

**힙**은 **max heap**과 **min heap**이 있다.

**max heap**

- 부모 노드의 키(key)가 자식 노드(들)의 키(key)보다 크거나 같은 트리

![](https://velog.velcdn.com/images/leequiett/post/86a1994a-cb56-4ea8-9c84-cbe7e7b1f692/image.png)

**min heap**

- 부모 노드의 키(key)가 자식 노드(들)의 키(key)보다 작거나 같은 트리
- min heap은 반대로 생각하면 된다.

---

**힙 주요 동작**

- insert → 힙에 아이템을 키값과 함께 넣어준다.(max heap 경우의 그림 예)

![](https://velog.velcdn.com/images/leequiett/post/cc69ad3d-8ba1-4cdb-a60b-f271a8e1f33d/image.png)

![](https://velog.velcdn.com/images/leequiett/post/a22de08d-8587-4ba2-869c-8e9fae7e26e3/image.png)

![](https://velog.velcdn.com/images/leequiett/post/505ea03c-5333-4d47-b749-84de83799265/image.png)

![](https://velog.velcdn.com/images/leequiett/post/03c77207-d85e-4b4c-9f5b-6885a529e3b5/image.png)

- delete → max heap의 경우 키값이 가장 큰 아이템을 꺼낸다.

![](https://velog.velcdn.com/images/leequiett/post/abed7a73-fb44-45b2-a9c1-ea5b24d5bc4b/image.png)

![](https://velog.velcdn.com/images/leequiett/post/cb44b1f5-3b68-487e-bdc5-4e811d0e2aea/image.png)

![](https://velog.velcdn.com/images/leequiett/post/debe2897-4497-41a5-8d96-040d86b1c010/image.png)

![](https://velog.velcdn.com/images/leequiett/post/39d8e573-4204-4fc2-a2bf-078fc2826342/image.png)

![](https://velog.velcdn.com/images/leequiett/post/cba23986-7264-4e69-8caa-2aeec43c4641/image.png)

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

heap의 사전적인 의미가 범위라는 뜻이라기 때문에 힙 메모리의 힙은 메모리에 쌓여있는 더미라고 이해하시면 된다.

- 참조
    - 쉬운코드. 우선순위 큐와 힙의 개념과 차이, 사용 사례를 설명합니다! 힙이 어떻게 동작하는지도 예를 통해 자세히 설명합니다!([https://youtu.be/P-FTb1faxlo?si=HHrxAZcOG4BVmZ0a](https://youtu.be/P-FTb1faxlo?si=HHrxAZcOG4BVmZ0a)) 
