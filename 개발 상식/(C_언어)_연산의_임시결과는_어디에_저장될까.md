```c
#include <stdio.h>

int add(int a, int b)
{

	int c = a + b;

	return c;
}

int main(void)
{
	int data = 0;
	data = add(1, 4);
	return 0;
}
```
위 코드에서 연산의 임시결과인 c변수의 값은 어디에 저장되어 있을까?

디스어셈블리로 위 코드를 보면
```c
int main(void)
{
00007FF792AE17E0  push        rbp  
00007FF792AE17E2  push        rdi  
00007FF792AE17E3  sub         rsp,108h  
00007FF792AE17EA  lea         rbp,[rsp+20h]  
00007FF792AE17EF  lea         rcx,[__EF1CCED5_returnToRegi@c (07FF792AF1008h)]  
00007FF792AE17F6  call        __CheckForDebuggerJustMyCode (07FF792AE1357h)  
	int data = 0;
00007FF792AE17FB  mov         dword ptr [data],0  
	data = add(1, 4);
00007FF792AE1802  mov         edx,4  //#1
00007FF792AE1807  mov         ecx,1  //#2
00007FF792AE180C  call        add (07FF792AE129Eh)  //#3
00007FF792AE1811  mov         dword ptr [data],eax  //#4
	return 0;
00007FF792AE1814  xor         eax,eax  
}
00007FF792AE1816  lea         rsp,[rbp+0E8h]  
00007FF792AE181D  pop         rdi  
00007FF792AE181E  pop         rbp  
00007FF792AE181F  ret  
```
#1~4번 주석을 보면 add 함수의 오른쪽 인수부터 레지스터에 저장하고(#1, #2) add함수를 콜하고(#3) 더블워드 포인터 data에 eax레지스터의 값을 넣는다.
코드를 보면 data에 들어갈 값은 add함수의 리턴값이니 이 값이 레지스터에 저장되어 있었다는 소린데.. 디스어셈블리를 또 보면

```c
#include <stdio.h>

int add(int a, int b)
{
00007FF792AE1780  mov         dword ptr [rsp+10h],edx  
00007FF792AE1784  mov         dword ptr [rsp+8],ecx  
00007FF792AE1788  push        rbp  
00007FF792AE1789  push        rdi  
00007FF792AE178A  sub         rsp,108h  
00007FF792AE1791  lea         rbp,[rsp+20h]  
00007FF792AE1796  lea         rcx,[__EF1CCED5_returnToRegi@c (07FF792AF1008h)]  
00007FF792AE179D  call        __CheckForDebuggerJustMyCode (07FF792AE1357h)  

	int c = a + b;
00007FF792AE17A2  mov         eax,dword ptr [b]  
00007FF792AE17A8  mov         ecx,dword ptr [a]  
00007FF792AE17AE  add         ecx,eax  
00007FF792AE17B0  mov         eax,ecx  
00007FF792AE17B2  mov         dword ptr [c],eax  

	return c;
00007FF792AE17B5  mov         eax,dword ptr [c]  //#5
}
00007FF792AE17B8  lea         rsp,[rbp+0E8h]  
00007FF792AE17BF  pop         rdi  
00007FF792AE17C0  pop         rbp  
00007FF792AE17C1  ret
```
#5번 주석을 보면 더블워드 포인터 c의 값을 eax 레지스터에 저장한다.
![](https://velog.velcdn.com/images/leequiett/post/17c04946-e4a4-4d2f-a36f-67bca69c0465/image.png)<br>
RAX(eax) 레지스터의 값을 보면 add 함수의 리턴값인 5가 저장되어있는걸 볼 수 있다.

>* 참고문헌
> 1. 최호성. 독하게 시작하는 C 프로그래밍. 인프런 (https://inf.run/iXxzY)
> 2. Brian W. Kernighan , Dennis M. Ritchie. 2016. C Programming Language. 휴먼싸이언스
