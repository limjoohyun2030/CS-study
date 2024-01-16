---

비선형적 자료구조이며 부모 → 자식처럼 계층적인 관계를 가지고 있는 자료구조이다. 

- 힙(Heap)을 구현하는 방법 중 하나가 트리이다.

[Heap](https://www.notion.so/Heap-c0e59e9919eb4143a83e5c44809215a5?pvs=21)

![](https://velog.velcdn.com/images/leequiett/post/ce8cea27-0c61-47c0-9ba0-1cf40f6cdda3/image.png)


> 이진 트리 예 ([https://pixabay.com/ko/vectors/데이터-구조-이진-트리-7390522/](https://pixabay.com/ko/vectors/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B5%AC%EC%A1%B0-%EC%9D%B4%EC%A7%84-%ED%8A%B8%EB%A6%AC-7390522/))
> 

## 구성요소 및 용어

---

- 노드(node) : 트리를 구성하는 항목
- 루트(root) : 트리의 최상위 노드
- 자식 노드(child node) : 루트 노드 아래의 노드
- 부모 노드(parent node) : 자식노드에 연결 되어있는 위 계층 노드
- 서브트리 (sub tree) : 부모 노드와 연결된 간선을 끊었을 때 생성되는 트리
- 간선 : 트리를 구성하기 위해 노드와 노드를 연결하는 선
- 진입 차수 : 어떤 노드에 대해 들어오는 간선의 개수
- 진출 차수 : 어떤 노드에서 나가는 간선의 개수
- 레벨 : 특정 깊이를 가지는 노드의 집합(루트는 레벨0)
- 높이 : 레벨 + 1

---

## 순회 방식

---

- 전위 순회 (pre order) : Parent → Left → Right <PLR>
    - 루트를 방문
    - 왼쪽 서브트리를 전위 순회로 순회
    - 오른쪽 서브트리를 전위 순회로 순회
- 중위 순회 (in order) : Left → Parent → Right <LPR>
    - 왼쪽 서브트리를 중위 순회로 순회
    - 루트를 방문
    - 오른쪽 서브트리를 중위 순회로 순회
- 후위 순회 (post order) : Left → Right → Parent <LRP>
    - 왼쪽 서브트리를 후위 순회로 순회
    - 오른쪽 서브트리를 후위 순회로 순회
    - 루트를 방문

## 트리 종류

---

### 이진 트리(binary tree)

**모든 노드의 차수가 2 이하**인 트리

이진 트리는 아래와 같이 분류할 수 있다.

- 전 이진 트리(full binary tree)
    - **모든 노드가 0또는 2개의 자식**을 가지는 트리
- 포화 이진트리(perfect binary tree)
    - 이진 트리의 **각 레벨이 허용되는 최대 개수 노드**를 가지는 트리

### 이진 탐색 트리(binary search tree, BS-Tree)

왼쪽 서브트리에 속한 노드의 key값은 부모 노드의 key값보다 작고 오른쪽 서브트리에 속한 노드의 key값은 부모 노드의 key값보다 큰 트리

![](https://velog.velcdn.com/images/leequiett/post/25e29ac8-a694-4f98-932a-54a79f171d49/image.png)

> [https://ko.wikipedia.org/wiki/이진_탐색_트리](https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%ED%83%90%EC%83%89_%ED%8A%B8%EB%A6%AC)
> 

### 스레드 이진 트리(threaded binary tree)

이진 트리를 순회할때 일반적으로 스택을 사용해 돌게 된다 그러나 함수 호출보다 포인터를 사용하는게 오버헤드가 적기 때문에 **포인터를 사용해 순회의 효율성을 높인 스레드 이진 트리**를 사용할 수 있다.

```cpp
//이진 검색 트리의 노드 구조체 및 삽입 연산

struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;
};

// 이진 검색 트리에 노드를 삽입하는 함수
TreeNode* insert(TreeNode* root, int key) {
    if (!root) {
        root = new TreeNode();
        root->data = key;
        root->left = root->right = nullptr;
    } else if (key < root->data) {
        root->left = insert(root->left, key);
    } else if (key > root->data) {
        root->right = insert(root->right, key);
    }
    return root;
}
```

```cpp
struct ThreadedNode {
    int data;
    ThreadedNode* left;
    ThreadedNode* right;
    bool leftThread;  // 왼쪽 포인터가 스레드인지 여부
    bool rightThread; // 오른쪽 포인터가 스레드인지 여부
};

// 스레드 트리에 노드를 삽입하는 함수
ThreadedNode* insert(ThreadedNode* root, int key) {
    ThreadedNode* newNode = new ThreadedNode();
    newNode->data = key;
    newNode->left = newNode->right = nullptr;
    newNode->leftThread = newNode->rightThread = true;

    if (!root) {
        root = newNode;
    } else {
        ThreadedNode* current = root;
        ThreadedNode* parent = nullptr;

        while (current) {
            parent = current;
            if (key < current->data) {
                if (!current->leftThread) {
                    current = current->left;
                } else {
                    break;
                }
            } else if (key > current->data) {
                if (!current->rightThread) {
                    current = current->right;
                } else {
                    break;
                }
            } else {
                delete newNode; // 중복된 키는 허용되지 않음
                return root;
            }
        }

        if (key < parent->data) {
            newNode->left = parent->left;
            newNode->right = parent;
            parent->left = newNode;
            parent->leftThread = false;
        } else {
            newNode->left = parent;
            newNode->right = parent->right;
            parent->right = newNode;
            parent->rightThread = false;
        }
    }

    return root;
}
```

### Splay, AVL, BB트리

경험적으로 좋은 성능의 BS 트리를 구축하는 방법

- 자주 탐색하는 키를 가진 노드를 트리의 **루트에 가깝게** 놓는다.
- 트리가 **균형이 되도록 유지**한다. 즉 각 노드에 대해 왼쪽과 오른쪽 서브트리가 가능한 한 같은 수의 노드를 갖게 한다.

### SPLAY 트리

자주 탐색하는 키를 가진 노드를 **루트에 가깝게 위치**하도록 구성한 BS트리

### AVL 트리

어떠한 노드를 기준으로 왼쪽 서브트리의 높이와 오른쪽 서브트리의 높이가 최대 1 만큼 차이가 나는 트리**(균형이 되도록 유지)**

![](https://velog.velcdn.com/images/leequiett/post/52357aa9-554b-4724-9d9e-f898a088a185/image.png)

> [https://ko.wikipedia.org/wiki/AVL_트리](https://ko.wikipedia.org/wiki/AVL_%ED%8A%B8%EB%A6%AC)
> 

### 멀티웨이 탐색 트리 → B트리

- 균형에 대해서는 특별히 제한하지 않는다.
- **각 노드가 자식을 많이 갖게 하여 트리의 높이를 줄인**다.

![](https://velog.velcdn.com/images/leequiett/post/d9399181-0a7b-418f-8733-a3b93759f797/image.png)

> [https://ko.wikipedia.org/wiki/B_트리](https://ko.wikipedia.org/wiki/B_%ED%8A%B8%EB%A6%AC)
> 

---

## 이진 트리 구현

---

### 배열에 저장

```jsx
       A
    ↙    ↘
   B        C
 ↙  ↘    ↙  ↘
D     E   F     G

완전 이진트리일 경우 메모리에 낭비가 없음 {A,B,C,D,E,F,G}
```

```jsx
       A
    ↙    ↘
   B        
 ↙  ↘    ↙  ↘
D             

메모리에 낭비가 큼 -> {A,B,null,D,null,null,null}
```

위 방법처럼 트리를 연속된 배열에 저장할 수 있다.

full binary tree 또는 perfect binary tree일 경우 낭비되는 공간이 없지만 그렇지 않다면 낭비되는 공간이 2^n에 비례해 늘어난다.

이러한 이유로 이진 트리는 보통 연결 리스트로 구현한다.

### 연결리스트로 저장

![](https://velog.velcdn.com/images/leequiett/post/cf5de146-0234-48e2-bf20-d66d0bf272a5/image.png)

위 그림과 같이 왼쪽 서브트리 포인터, 데이터, 오른쪽 서브트리 포인터로 구성한다.

---

- 참조
    - Crocus. 스레드 이진 트리(Thread Binary Tree) 개념(1)(https://www.crocus.co.kr/366)
    - 강태원, 정광식. 2023. 자료구조. 2개정판. 서울 : 한국방송통신대학교 출판문화원
    - 권오병, 박미경. 2002. 자료구조. 서울 : 성안당