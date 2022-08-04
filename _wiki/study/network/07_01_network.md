---
layout  : wiki
title   : 네트워크 주소 변환 기술(NAT / PAT / Source NAT / Destination NAT / Dynamic NAT / Static NAT)
summary : 외부망과 내부망을 분리하고 공인 IP 없이 내부 네트워크와 외부 네트워크를 간접적으로 연결하는 방법
date    : 2021-11-28 09:00:00 +0900
updated : 2021-11-28 09:00:01 +0900
tag     : network
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![NAT](https://user-images.githubusercontent.com/65143458/141114269-ff873e5d-40ec-4937-a516-ee27015bb45d.jpeg)

_↑↑↑ NAT_

![PAT](https://user-images.githubusercontent.com/65143458/141115113-87d49283-a42c-48f5-a6a8-946edababb25.png)

_↑↑↑ PAT_

## 개 요

## NAT (Network Address Translation)

* 네트워크 주소 변환 기술

* 라우터(L3 스위치), 로드 밸런서 / 방화벽 / 세션 장비(L4 스위치)에서 사용

* 주소 변환 방식 

    * 1 : 1 변환

    * 여러 개의 IP를 하나의 IP로 변환 (Network Address Port Translation)

    * IPv4 주소를 IPv6 주소로 변환 (Address Family Translation)

<br/>

## NAT / PAT 용도와 필요성

* 한정된 IPv4 주소 자원 해결: 외부에 공개해야 하는 서비스는 공인 IP, 그렇지 않은 경우 사설(내부) IP 사용

* 주소 은닉 및 보안 강화: 공개가 필요한 서비스의 사설(내부) IP를 직접 공개하지 않고 변환된 외부 IP 또는 포트로 공개

<br/>

## NAT 동작 방식

* SNAT (Source NAT)

    * 내부 사설 망에서 외부 공인망으로 통신할 때 사용

    * 공인 IP 주소로부터 응답을 받으려면 출발지 IP 주소가 공인 IP여야 하므로, 출발지(Source)의 사설 IP를 공인 IP로 주소 변환 

* DNAT (Destination NAT)

    * 로드 밸런서의 부하 분산 또는 대외망과의 네트워크 구성시 사용

    * 로드밸런서: 사용자가 요청한 VIP(Virtual IP) 주소를 실제 목적지(Destination) 서버의 IP로 주소 변환

* Static NAT (1:1 NAT)

    * 가장 쉬운 변환 방법으로 사설 IP 개수만큼 공인 IP가 필요함

    * 사설 대역의 서버가 역할이 많아 포트포워딩 작업이 많이 필요한 경우

* Dynamic NAT (Dynamic NAT)

    * 공인 IP가 사설 IP 보다 적을 경우 사용

    * 적은 수의 공인 IP를 효율적으로 사용할 필요가 있는 경우

    * 내부 네트워크의 사용 패턴을 외부에 숨겨야 할 필요가 있는 경우

<br/>

