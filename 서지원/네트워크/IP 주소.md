# Chapter2 네트워크

<br>

# 📚 목차

> **[1. ARP](#arpaddress-resolution-protocol)**
>
> **[2. 홉바이홉 통신](#홉바이홉-통신)**
> 
> **[3. IP 주소 체계](#ip-주소-체계)**
> 
> **[4. IP 주소를 이용한 위치 정보](#ip-주소를-이용한-위치-정보)**


<br>

## ARP(Address Resolution Protocol)

IP 주소로부터 **MAC 주소를 구하는** `IP`와 **`MAC 주소`의 다리 역할**을 하는 프로토콜이다. 

![Image](https://github.com/user-attachments/assets/b09870e3-5cbb-4f67-9d1c-62aa240f2359)

<br>

> 📕 **용어**
> 
> **브로드캐스트**
> 
> **송신 호스트**가 전송한 `데이터`가 <ins>네트워크에 연결된 모든 호스트에 전송</ins>되는 방식
> 
> **유니캐스트**
> 
> 고유 식별자로 식별된 **하나의 네트워크 목적지에 1:1로 데이터를 전송**하는 방식

<br>

**[ARP의 주소 찾는 과정]**

![Image](https://github.com/user-attachments/assets/281e2b5c-ccb6-4bf5-b285-2b7adcc2a600)

1️⃣ A가 요청한 **IP주소에 해당하는 MAC 주소**를 찾기

2️⃣ 해당 장치(B)는 `유니캐스트`를 통해 **MAC 주소를 반환**

➡️ IP 주소에 맞는 MAC 주소를 찾는다. 


<br>

## 홉바이홉 통신

IP 주소를 통해 통신하는 과정을 의미한다. 

> 💡 **홉(Hop)이란 영어 뜻 자체로는 건너뛰는 모습을 의미한다. 
> 이는 통신망이서 각 패킷이 여러개의 라우터를 건너가는 모습을 비유적으로 표현한 것이다.** 


<br>

**[동작 방식]**

`서브네트워크(통신 장치)`에 있는 라우터의 `라우팅 테이블`의 **IP를 기반**으로 다음 IP로 이동하는 <ins>라우팅을 수행</ins>하며 **최종 목적지까지 패킷을 전달하는 통신**을 의미한다. 

![Image](https://github.com/user-attachments/assets/99ef0fae-ca20-4da1-bfea-130793791d05)

> 💡 **라우팅**
> 
> IP 주소를 찾아가는 과정


<br>

### 라우팅 테이블 (Routing Table)

- 송신지에서 수신지까지 도달하기 위해 사용된다. 
- 라우터에 들어가 있는 **목적지 정보들**과 그 목적지로 **가기 위한 방법**이 들어 있는 <ins>리스트를 의미</ins>한다. 
- **게이트웨이**, 다음 목적지의 **라우터 정보**를 가지고 있다. 


<br>

### 게이트웨이 (Gateway)

서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 **통신을 가능하게 하는 관문 역할**을 하는 `컴퓨터`나 `소프트웨어`를 말한다. 

**ex> 톨게이트**

- 게이트웨이는 서로 다른 네트워크상의 **통신 프로토콜을 변환해주는 역할**을 하기도 한다. 
- **게이트웨이는 라우팅 테이블**을 통해 볼 수 있다.

> 💡 **라우팅 테이블 보기**
> 
> 윈도우: **netstat -r**
> 
> ![Image](https://github.com/user-attachments/assets/e5c6c5dc-a740-4794-b71a-6b7e92c9066c)


<br>

## IP 주소 체계

### IPv4

- 32비트를 8비트 단위로 점을 찍어 표기한다. ➡️ **123.45.67.89**



### IPv6 

- 64비트를 16비트 단위로 점을 찍어 표기한다. ➡️ **2001:db8::ff00:42:8329**

<br>

#### 클래스 기반 할당 방식 (Classful network addressing) - 초기 방법

![Image](https://github.com/user-attachments/assets/0810252a-13b0-44ee-9fcc-c80d866c87ef)

- A, B, C, D, E 다섯 개의 클래스로 구분하는 방식
- 앞단:  네트워크 주소
- 뒷단: 호스트 주소


`A, B, C` ➡️ **일대일 통신**에 사용

`D` ➡️ **멀티캐스트** 통신

`E` ➡️ **예비**용


<br>

**[주소 범위]**

![Image](https://github.com/user-attachments/assets/f6be36bb-6b39-4e76-87e0-db801496ff8c)

<br>

**[네트워크 주소와 브로드캐스트용 주소]**

![Image](https://github.com/user-attachments/assets/3451fd8f-eff8-4a1f-ae28-96d12f465eff)

<br>

**⚠️ 해당 방식은 사용하는 주소보다 버리는 주소가 많은 단점이 있다.**  ➡️ **DHCP, IPv6, NAT으로 해소**

<br>

### DHCP (Dynamic Host Configuration Protocol)

**IP 주소** 및 기타 **통신 매개변수**를 <ins>자동으로 할당</ins>하기 위한 네트워크 관리 프로토콜이다. 

➡️ `인터넷을 접속할 때`마다 **자동으로 IP 주소를 할당**할 수 있다. 

![Image](https://github.com/user-attachments/assets/241a18e8-df1d-41cd-9769-5e095a9effa9)

<br>

**[사용 예시]**

많은 라우터와 게이트웨이 장비에 포함되어 있고 이를 통해 **가정용 네트워크에서 IP주소를 할당**한다. 



<br>

### NAT (Network Address Translation)

`패킷`이 **라우팅 장치를 통해 전송**되는 동안 <ins>패킷의 IP주소 정보를 수정하여 IP주소를 다른 주소로 매핑</ins>하는 방법

➡️ IPv4로 감당하지 못하는 많은 주소들을 **NAT**을 통해 `공인IP`와 `사설I`P로 나눠서 **많은 주소들을 처리**한다. 

> 💡 **NAT을 가능하게 하는 소프트웨어: ICS, RRAS, Netfilter**


<br>

**[NAT 사용 흐름]**

![Image](https://github.com/user-attachments/assets/bc4a2fd5-bf0e-4840-933b-3dff9dac629c)

이미지와 같이 **하나의 라우터로 여러 PC를 연결할 수 있는 이유**는 해당 공유기에 `NAT 기능이 탑재`되어 있기 때문이다. 

<br>

#### NAT을 이용한 보안 

**내부 네트워크에서 사용하는 IP 주소**와 **외부에 들어나는 IP 주소**를 다르게 유지할 수 있어 내부 네트워크에 대해 <ins>어느 정도의 보안이 가능</ins>하다.  

<br>

#### NAT 단점

**여러 명이 동시에 접속**하기 때문에 실제로 접속하는 호스트 숫자에 따라서 <ins>접속 속도가 느려질 수 있다. 

<br>.

## IP 주소를 이용한 위치 정보

IP 주소를 통해 동 또는 구까지 위치 추적이 가능하다. 

> 💡 https://mylocation.co.kr/