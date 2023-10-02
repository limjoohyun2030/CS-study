# Stack

**ADT(abstract data type)**

- 추상자료형
- 개념적으로 어떤 동작이 있는지만 정의
- 구현에 대해서는 다루지 않음

**DS(data structure)**

- 자료구조
- ADT에서 정의된 동작을 실제로 구현한 것

### 개념

---

스택은 입력이 가장 늦게 된 자료가 가장 먼저 출력되는 LIFO(Last In First Out) 구조입니다. 

### 주요 동작

---

1. **Push()**: 새로운 데이터를 스택에 추가하는 작업. 이때 데이터는 스택의 맨 위에 추가.
2. **Pop()**: 스택에서 데이터를 꺼내는 작업. 스택의 맨 위에 있는 데이터가 꺼내지며, 스택에서 제거. 
3. **Peek()**: 스택의 맨 위에 있는 데이터를 조회하지만 제거하지 않는 작업. 이를 통해 스택의 맨 위 데이터를 확인.
4. **isEmpty()**: 스택이 비어 있는지 확인하는 작업. 스택이 비어 있으면 데이터를 꺼낼 수 없으므로 주로 스택이 비어 있는지 검사하는 용도로 사용.

### 스택 사용 사례

---

1. **stack memory & stack frame - 함수 호출과 복귀 관리**
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/blob/main/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/Stack/Untitled.png?raw=true)
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/blob/main/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/Stack/Untitled%201.png?raw=true)
    
    **Step 1.** 프로그램이 실행되면, 가장 먼저 main() 함수가 호출되어 main() 함수의 스택 프레임이 스택에 저장됩니다.
    
    **Step 2.** func1() 함수를 호출하면 해당 함수의 매개변수, 반환 주소값, 지역 변수 등의 스택 프레임이 스택에 저장됩니다.
    
    **Step 3.** func2() 함수를 호출하면 해당 함수의 스택 프레임이 추가로 스택에 저장됩니다.
    
    **Step 4.** func2() 함수의 모든 작업이 완료되어 반환되면, func2() 함수의 스택 프레임만이 스택에서 제거됩니다.
    
    **Step 5.** func1() 함수의 호출이 종료되면, func1() 함수의 스택 프레임이 스택에서 제거됩니다.
    
    **Step 6.** main() 함수의 모든 작업이 완료되면, main() 함수의 스택 프레임이 스택에서 제거되면서 프로그램이 종료됩니다.
    
    이처럼 스택은 가장 나중에 저장된 데이터가 가장 먼저 인출되는 방식으로 동작합니다.
    
    이러한 방식을 후입선출(LIFO, Last-In First-Out) 방식이라고 합니다.
    
    이때 스택은 푸시(push) 동작으로 데이터를 저장하고, 팝(pop) 동작으로 데이터를 인출합니다.
    

### 스택 오버플로우(stack overflow)

![Untitled](https://github.com/limjoohyun2030/CS-study/blob/main/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/Stack/Untitled%202.png?raw=true)

함수의 재귀 호출이 무한히 반복되면, 해당 프로그램은 스택 오버플로우(stack overflow)에 의해 종료됩니다.

만약 재귀 호출이 무한히 반복되면, 위 그림에서 Step 3 이후로는 재귀 호출에 의한 스택 프레임이 계속해서 쌓여만 갈 것입니다.

이렇게 스택의 모든 공간을 다 차지하고 난 후 더 이상의 여유 공간이 없을 때 또 다시 스택 프레임을 저장하게 되면, 해당 데이터는 스택 영역을 넘어가서 저장되게 됩니다.

이렇게 해당 스택 영역을 넘어가도 데이터가 저장될 수 있으면, 해당 프로그램은 오동작을 하게 되거나 보안상의 크나큰 취약점을 가지게 됩니다.

1. **브라우저의 뒤로 가기 기능**
2. **문법 분석**
3. **후위 표기법 계산**
4. **미로 찾기**
5. **실행 취소 및 다시 실행**

### 구현 예

---

```java
public class CustomStack {
    private int maxSize;
    private int[] stackArray;
    private int top;

    public CustomStack(int size) {
        this.maxSize = size;
        this.stackArray = new int[maxSize];
        this.top = -1;
    }

    public void push(int value) {
        if (isFull()) {
            System.out.println("Stack is full. Cannot push " + value);
            return;
        }
        stackArray[++top] = value;
    }

    public int pop() {
        if (isEmpty()) {
            System.out.println("Stack is empty. Cannot pop.");
            return -1;
        }
        return stackArray[top--];
    }

    public int peek() {
        if (isEmpty()) {
            System.out.println("Stack is empty. Cannot peek.");
            return -1;
        }
        return stackArray[top];
    }

    public boolean isEmpty() {
        return top == -1;
    }

    public boolean isFull() {
        return top == maxSize - 1;
    }

    public int size() {
        return top + 1;
    }

    public static void main(String[] args) {
        CustomStack stack = new CustomStack(5);

        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println("Top element: " + stack.peek());
        System.out.println("Popped element: " + stack.pop());

        System.out.println("Is stack empty? " + stack.isEmpty());
        System.out.println("Stack size: " + stack.size());
    }
}
```

- 참조
    - 강태원, 정광식. 자료구조. 2개정판 1쇄. 서울 : 방송통신대학교 출판문화원.
    - TCPschool. 스택 프레임([http://www.tcpschool.com/c/c_memory_stackframe](http://www.tcpschool.com/c/c_memory_stackframe))
    - 쉬운코드. 스택과 큐([https://www.youtube.com/watch?v=-2YpvLCT5F8](https://www.youtube.com/watch?v=-2YpvLCT5F8))
