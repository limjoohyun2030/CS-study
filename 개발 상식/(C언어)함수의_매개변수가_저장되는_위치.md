### 결론
1. 32bit 환경 : Stack 영역
2. 64Bbit 환경 : Restier 영역
---
### 정말 그런지 확인해 보자!
우선 코드는 이렇다.
```c
#include <stdio.h>

int testFunc(int a, int b)
{
	int c = 1;

	return 0;
}

int main(void) 
{
	testFunc(1, 9);

	return 0;
}
```
### 위 코드를 64bit로 실행 시킨 뒤 디스어셈블리로 보면
```c
--- C:\Users\LeeQuiett\Desktop\Repo\HelloWorld\_12_args\args.c -----------------

int main(void) 
{
00007FF77CC91780  push        rbp  
00007FF77CC91782  push        rdi  
00007FF77CC91783  sub         rsp,0E8h  
00007FF77CC9178A  lea         rbp,[rsp+20h]  
00007FF77CC9178F  lea         rcx,[__401B9164_args@c (07FF77CCA1008h)]  
00007FF77CC91796  call        __CheckForDebuggerJustMyCode (07FF77CC91357h)  
	testFunc(1, 9);
00007FF77CC9179B  mov         edx,9  // #1
00007FF77CC917A0  mov         ecx,1  // #2
00007FF77CC917A5  call        testFunc (07FF77CC91104h)  

	return 0;
00007FF77CC917AA  xor         eax,eax  
}
00007FF77CC917AC  lea         rsp,[rbp+0C8h]  
00007FF77CC917B3  pop         rdi  
00007FF77CC917B4  pop         rbp  
00007FF77CC917B5  ret  
--- 소스 파일이 없습니다. ---------------------------------------------------------------


--- C:\Users\LeeQuiett\Desktop\Repo\HelloWorld\_12_args\args.c -----------------
#include <stdio.h>

int testFunc(int a, int b)
{
00007FF77CC917D0  mov         dword ptr [rsp+10h],edx  
00007FF77CC917D4  mov         dword ptr [rsp+8],ecx  
00007FF77CC917D8  push        rbp  
00007FF77CC917D9  push        rdi  
00007FF77CC917DA  sub         rsp,108h  
00007FF77CC917E1  lea         rbp,[rsp+20h]  
00007FF77CC917E6  lea         rcx,[__401B9164_args@c (07FF77CCA1008h)]  
00007FF77CC917ED  call        __CheckForDebuggerJustMyCode (07FF77CC91357h)  
	int c = 1;
00007FF77CC917F2  mov         dword ptr [c],1  //#3

	return 0;
00007FF77CC917F9  xor         eax,eax  
}
00007FF77CC917FB  lea         rsp,[rbp+0E8h]  
00007FF77CC91802  pop         rdi  
00007FF77CC91803  pop         rbp  
00007FF77CC91804  ret  
--- 소스 파일이 없습니다. ---------------------------------------------------------------
```
**#1, #2 주석**을 보면 9를 edx 레지스터에, 1을 ecx 레지스터에 저장하는 연산을 수행함을 알 수 있다. 이 때 **매개변수는 오른쪽의 매개변수 부터 저장한다.**
이 때 중단점을 걸고 레지스터를 보면 ![](https://velog.velcdn.com/images/leequiett/post/1c0c3389-e267-4e11-9453-18c45a7fe96b/image.png)RDX(edx)에 9를 RCX(ecx)에 1를 저장한 걸 볼 수 있다. 


**#3 주석**을 보면 더블워드에 대한 포인터가 가리키는 주소에다가 c를 저장하는 연산을 수행한다. 아마 stack 영역 어딘가에 있을것이다. (자동변수는 stack 영역을 사용하기 때문)
이 때 중단점을 걸고 메모리를 보면
![](https://velog.velcdn.com/images/leequiett/post/bd152fdf-910c-4c32-88ac-6a18e0070e0a/image.png) 메모리에 1이 저장된걸 볼 수 있다.

---

### 위 코드를 32bit로 실행 시킨 뒤 디스어셈블리로 보면
```c
--- C:\Users\LeeQuiett\Desktop\Repo\HelloWorld\_12_args\args.c -----------------

int main(void) 
{
00CA1780  push        ebp  
00CA1781  mov         ebp,esp  
00CA1783  sub         esp,0C0h  
00CA1789  push        ebx  
00CA178A  push        esi  
00CA178B  push        edi  
00CA178C  mov         edi,ebp  
00CA178E  xor         ecx,ecx  
00CA1790  mov         eax,0CCCCCCCCh  
00CA1795  rep stos    dword ptr es:[edi]  
00CA1797  mov         ecx,offset _401B9164_args@c (0CAC008h)  
00CA179C  call        @__CheckForDebuggerJustMyCode@4 (0CA1320h)  
	testFunc(1, 9);
00CA17A1  push        9  //#1
00CA17A3  push        1  //#2
00CA17A5  call        _testFunc (0CA12F8h)  
00CA17AA  add         esp,8  

	return 0;
00CA17AD  xor         eax,eax  
}
00CA17AF  pop         edi  
00CA17B0  pop         esi  
00CA17B1  pop         ebx  
00CA17B2  add         esp,0C0h  
00CA17B8  cmp         ebp,esp  
00CA17BA  call        __RTC_CheckEsp (0CA123Fh)  
00CA17BF  mov         esp,ebp  
00CA17C1  pop         ebp  
00CA17C2  ret  
--- 소스 파일이 없습니다. ---------------------------------------------------------------
```
**#1, #2 주석**을 보면 push연산을 하는데 이렇게 
```c
push ax
```
식으로 사용하면 ax의 값을 stack 영역에 넣는 연산이다. 따라서 매개변수를 64bit 환경과 다르게 stack 영역에 저장함을 볼 수 있다.

자동변수에 대한 설명은 생략하겠다.

### 결론
32bit와 64bit 환경에서 매개변수가 저장되는 위치는 다르다.

>* 참조<br>
최호성. 독하게 시작하는 C 프로그래밍. 인프런 (https://inf.run/iXxzY)
