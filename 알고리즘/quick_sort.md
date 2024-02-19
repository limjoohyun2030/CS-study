퀵 정렬이 무엇인지는 위키백과 링크로 대체한다.
>https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC

```
#include <stdio.h>

void quicSort2(int* array, int L, int R);

int* quicSort(int* array, int size) {
    quicSort2(array, 0, size - 1);
    return array;
}

void quicSort2(int* array, int L, int R) {
    int pivot = (L + R) / 2; // pivot == 3;
    if (L >= R) { // 배열의 크기가 1 이하인 경우 종료
        return;
    }

    int i = L, j = R;
    int temp = 0;

    while (i <= j) {
        while (array[i] < array[pivot]) {
            i++;
        }
        while (array[j] > array[pivot]) {
            j--;
        }
        if (i <= j) {
            temp = array[i];
            array[i] = array[j];
            array[j] = temp;
            i++;
            j--;
        }
    }

    // 재귀호출
    quicSort2(array, L, j);
    quicSort2(array, i, R);
}

int main(void) {
    int array[] = { 5, 1, 6, 3, 4, 2, 7 };
    int size = sizeof(array) / sizeof(array[0]); // size == 7

    quicSort(array, size);

    for (int i = 0; i < 7; i++)
    {
        printf("%d ", array[i]);
    }

    return 0;
}
```
