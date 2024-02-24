버블 정렬, 선택 정렬이 무엇인지는 위키백과 링크로 대체한다 
>버블 정렬 https://ko.wikipedia.org/wiki/%EB%B2%84%EB%B8%94_%EC%A0%95%EB%A0%AC

>선택 정렬 https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC


### 버블 정렬
```c
#include <stdio.h>

int main(void) {
    int array[5] = {20, 10, 5, 70, 32};

    for (int i = 0; i < 4; i++) { // 배열의 마지막 요소는 자동으로 정렬되므로 마지막 요소 전까지만 반복
        for (int j = i + 1; j < 5; j++) {
            if (array[i] > array[j]) {
                int temp = array[i];
                array[i] = array[j];
                array[j] = temp;
            }
        }
    }

    for (int i = 0; i < 5; i++) printf(" %d ", array[i]);

    return 0;
}
```
위의 코드는 간단하게 버블 정렬을 구현한 것이지만, 비효율적인 부분이 있다. 바로 인접한 두 요소를 비교할 때마다 교환 연산을 수행하는 부분이다. 이 연산은 불필요한 메모리 접근을 유발할 수 있다. 따라서 이를 최적화할 필요가 있다.

### 선택정렬

```c
#include <stdio.h>

int main(void) {
    int array[5] = {20, 10, 5, 70, 32};

    for (int i = 0; i < 5; i++) { // 배열의 마지막 요소는 자동으로 정렬되므로 마지막 요소 전까지만 반복
        int min = i;
        for (int j = i + 1; j < 5; j++) {
            if (array[min] > array[j]) {
                min = j;
            }
        }
        if (min != i) {
            int temp = array[i];
            array[i] = array[min];
            array[min] = temp;
        }
    }

    for (int i = 0; i < 5; i++) printf(" %d ", array[i]);

    return 0;
}
```
위의 선택정렬 알고리즘 에서는 교환 연산을 필요할 때만 수행하여 불필요한 오버헤드를 줄일 수 있도록 작성하였다.
