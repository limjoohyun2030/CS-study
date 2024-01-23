TCP를 자세히 살펴보기 전에 TCP에서의 패킷을 세그먼트라고 한다.

# TCP 오류제어(Error Control)

신뢰성 있는 서비스를 제공하기 위하여, TCP는 오류 제어 매커니즘을 구현한다. 오류 제어에서는 세그먼트가 (손실 또는 훼손된 세그먼트 등의) 오류 감지를 위한 데이터의 단위이기는 하지만, 오류 제어는 바이트-단위로 동작한다.

## ARQ(Automatic Repeat Request)

통신 중 요류 발생 시 송신측에서 수신측으로 데이터를 재전송하는 방식

재전송은 그 자체로 비효율적이기 때문에 이러한 재전송을 최대한 적게하는 방식을 사용해야한다.

## Stop-and-wait

이름 그대로 멈추고 기다리는 방식이다. 타임아웃이 되어도 ACK를 받지 못하면 패킷을 재전송하는 방식이다.

동작을 설명하자면 다음과 같다. 

A에서 0번 패킷을 전송했다고 가정한다. B에서는 0번 패킷을 수신하고 그에 대한 ACK로 응답한다. 

B는 ACK를 받으면 1번 패킷을 전송한다. 하지만 1번 패킷이 손실되어 A에 도달하지 못하면 타이머가 만료되어 1번 패킷을 재전송한다.

이와 같은 방식으로 매번 전송한 패킷에 대한 ACK를 받고 다음 패킷을 전송하는 방식이 STOP-AND-WAIT 방식이다.

![](https://velog.velcdn.com/images/leequiett/post/8ff36207-fb2b-4183-8da7-d730f06bb22e/image.png)

## Go Back N

GO-BACK-N 재전송 방식은 어느 한 패킷이 돌아오지 않아 에러가 발생한 경우, 그 번호부터 다시 재전송하는 방식이다.

동작을 설명하면 다음과 같다. 

A에서 0번, 1번, 2번 패킷을 전송한다고 가정한다. 이중 0번, 1번, 2번 패킷은 ACK를 수신하여 버퍼에 저장하고 ACK 3을 전송해 3부터 전송하면 된다고 알린다. 

송신측이 3~5 패킷을 보냈는데 4번 패킷이 유실되어 수신측이 받지 못해 타이머가 만료되었고 수신측은 에러가 발생한 4번 패킷 이후의 데이터는 모두 폐기하고 ACK4를 보내 4번 패킷 이후로 다시보내라는 신호를 보낸다.

송신측은 4번부터 다시 보낸다.

이와 같이 뒤로 돌아가서 다시 송신하는 방식이 GO-BACK-N 재전송 방식이다.

단점으로는 이미 수신한 데이터를 폐기하고 다시 재전송해야 한다는 점이다.

![](https://velog.velcdn.com/images/leequiett/post/9b83591f-2410-4a54-9d58-5dc239e057b7/image.png)

## Selective Repeat

마지막으로 SELECTIVE REPEAT 재전송 방식은 오류가 발생한 패킷만 선택적으로 재전송하는 방식이다.

위와 동일하게 4번 패킷에서 오류가 발생했고 수신측이 송신측에 ACK를 보내는데 수신측에서 5번 패킷은 폐기하지 않는다.

즉 송신측은 오류가 발생한 4번 패킷만은 재전송하고 수신측이 이를 정상적으로 받으면 다시 6번 패킷부터 전송을 시작한다.

이 때 수신측의 버퍼에 순서가 보장되지 않기 때문에 수신 버퍼에 대한 재정렬이 필요하며 이는 필연적으로 또 다른 버퍼 공간을 필요로 한다는 단점이 있다.

![](https://velog.velcdn.com/images/leequiett/post/c496901a-ea35-4b83-ae4a-a8191c82c370/image.png)

---

# TCP 흐름제어(Flow Control)

TCP는 UDP와는 다르게 흐름 제어를 제공한다. 송신 TCP는 송신 프로세스로부터 수신되는 데이터의 양을 조절하며, 수신 TCP는 송신 TCP로부터 전송되는 데이터의 양을 조절한다. 이것은 수신측에서 데이터가 과도하게 수신됨으로 인한 데이터의 손실을 방지하기 위한 것이다. TCP는 바이트-단위의 흐름 제어를 위하여 번호화 시스템을 이용한다.

## 흐름제어 기법

## Stop-and-wait

전송한 패킷에 대한 확인 응답을 받아야만 그 다음 패킷을 전송하는 기법

![](https://velog.velcdn.com/images/leequiett/post/24869a5a-8c24-4d88-9c35-4677cf8f841f/image.png)

## Sliding Window(윈도우 광고 기법)

각 송수신지에 있는 슬라이딩 윈도우를 활용하는 방식으로 계속해서 자신의 윈도우 크기(Window Size)를 상대에게 알려주는 “윈도우 광고 기법”을 사용한다.

수신측(Receiver window, rwnd)과 송신측(Congestion window, cwnd)의 윈도우 크기를 서로 알리면서 윈도우 크기가 변화하기도 한다.

**흐름제어 과정**

B는 A에게 자신의 윈도우 크기를 알림으로써 Window 크기가 250이라는 정보를 보낸다.

A가 B에게 데이터를 전송하고자 한다. A의 윈도우 크기도 250이며 이중 100 바이트의 데이터를 B에게 전송하고자 한다.

데이터를 수신받은 B는 받은 100바이트의 데이터를 버퍼에 저장한다. 따라서 B의 윈도우 크기는 150이 된다.

그리고 확인 응답 ACK=101을 전송하며 윈도우 크기가 150임을 알린다.

확인 응답 ACK=101을 받은 송신지 A의 윈도우 크기는 B와 같은 150이 된다.

송신지 윈도우 크기는 수신지 윈도우 크기에 맞춰서 상대방이 받을 수 있도록 크기를 맞춘다.

A가 B에게 또 데이터를 전송하고자 하면 A의 윈도우 크기는 150이며, 이중 50바이트의 데이터를 B에게 전송한다

B는 확인 응답 ACK = 151을 전송하며, 자신의 윈도우 크기가 150임을 알린다.

ACK = 151과 B의 윈도우 크기가 150이라는 정보를 수신한 송신측 윈도우도 그에 따라 윈도우를 조절한다.

![](https://velog.velcdn.com/images/leequiett/post/a768329d-30fe-4dd7-b623-61004d3e0ff3/image.png)

---

# TCP 혼잡 제어**(Congestion Control)**

TCP는 UDP와는 다르게 망의 혼잡을 고려한다. 송신측에서 전송되는 데이터의 양은(흐름 제어에 의해서) 수신측에 의해서 조절될 뿐만 아니라 망의 혼잡 정도에 의해서도 결정된다.

**TCP가 혼잡 상황을 인식하는 경우 :** 

1. 데이터를 송신하고 타임아웃되어 재전송해야 할 경우
2. 3개 이상의 중복된 ACK 수신

**혼잡 제어 방식 :**

- **혼잡 발생 전**
    - **Slow Start** 
        - TCP는 Slow Start 방식으로 하나씩 전송한다. 이 때 cwnd의 값은 1로 시작하고 1, 2, 4, 8의 지수적 증가 방식을 통해 세그먼트를 임계값까지 전송한다.
    - **Additive Increase**
        - Ssthresh값까지 cwnd가 도달하게 되면 TCP는 혼잡 상황으로 갈 위험이 높다고 여겨져 Additive Increase 기법을 사용해 혼잡을 회피한다.
        - cwnd를 하나씩 증가시키며 혼잡을 회피한다
- **혼잡 발생 후**
    - **Multiplicative Decrease**
        - 결국 혼잡이 발생되면 Multiplicative Decrease 방식을 수행, 혼잡이 발생한 상황의 경우에 따라 다르게 동작
        - **세그먼트 전송 후 타임아웃되어서 재전송해야 하는 상황** : cwnd의 크기는 1로 재설정 되고 ssthresh값은 20의 1/2인 10으로 변경된다. 이후에는 Slow Start와 Additive Increase 기법을 계속해서 수행.
                
             
             ![](https://velog.velcdn.com/images/leequiett/post/a61777d0-47f6-46bb-b38e-004acf7df5f3/image.png)

                
         - **3개 이상의 중복된 ACK를 수신한 경우 :** ssthresh값이 1/2로 축소, cwnd의 크기는 ssthresh와 같은 값으로 재설정, 이후에는 Additive Increase 기법으로 혼잡 회피만 수행
                
              ![](https://velog.velcdn.com/images/leequiett/post/618a61fd-6f7f-40bb-99fb-4bc074542122/image.png)

                

- 참조
    - 예술하는 개발자. 네트워크, 그림으로 이해하자([https://inf.run/21VV](https://inf.run/21VV))
    - B. ehrouz A. Forouzan. 2014. TCP / IP Protocol Suite. 4쇄. Mc Graw Hill Education.
    - nnnyeong. [Network] TCP 오류제어([https://velog.io/@nnnyeong/Network-TCP-오류제어](https://velog.io/@nnnyeong/Network-TCP-%EC%98%A4%EB%A5%98%EC%A0%9C%EC%96%B4))
    - 여니드. [Network] TCP/IP 흐름제어, 혼잡제어([https://yeoneeds.tistory.com/25](https://yeoneeds.tistory.com/25))
    - 해로. TCP 흐름제어 기법 살펴보기([https://velog.io/@haero_kim/TCP-흐름제어-기법-살펴보기](https://velog.io/@haero_kim/TCP-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4-%EA%B8%B0%EB%B2%95-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0))
