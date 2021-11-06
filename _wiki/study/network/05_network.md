---
layout  : wiki
title   : 라우터(3계층 장비)
summary : 
date    : 2021-11-05 09:00:00 +0900
updated : 2021-11-05 09:00:01 +0900
tag     : network
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![router](https://user-images.githubusercontent.com/24386223/139842064-4aed90a7-4728-47af-9409-ff25e74f33d9.png)

## 개 요

* 네트워크와 네트워크의 통신을 중계하는 장비

## 네트워크 트래픽 중계 장비와 역할/기능

* 스위치

    * IP 주소를 읽지 않으므로, 외부와의 연결은 불가능함

<br/>

* 라우터

    * 라우터는 IP 주소를 기반으로 하나의 네트워크에서 다른 네트워크로 데이터를 중계하는 기능 역할을 수행함

    * 데이터 패킷을 수신시에 목적지 IP를 검사한 뒤 내부 네트워크로 전달하고 그렇지 않으면 다른 네트워크로 전달함

    
## 라우터의 동작


* 경로 선택

    * 라우터는 경로 정보를 수집하여 라우팅 테이블을 구성함

    * 패킷을 수신하면 도착지 IP 주소와 라우팅 테이블 정보 확인한 후 경로 지정하여 패킷을 전달함

<br/>

* 브로드캐스트 컨트롤

    * 불필요한 통신 자원 낭비를 방지하기 위해, 도착지 정보가 확실한 경우에만 패킷을 전달함

    * 라우터는 브로드캐스트 패킷을 전달하지 않으므로 브로드캐스트가 많이 발생하는 네트워크는 라우터로 분할하기도 함


## 라우티드 프로토콜(Routed Protocol) vs 라우팅 프로토콜(Routing Protocol)

* 라우티드 프로토콜

    * 전송되는 데이터의 형식을 정한 교환 규칙

    * TCP / IP, IPX, AppleTalk 

<br/>

* 라우팅 프로토콜

    * 네트워크 또는 라우터 간에 데이터를 전달하는 방법

    * 자율 시스템(Autonomous System)

        * 여러 내부 라우팅 프로토콜(IGP)을 동일한 정책으로 관리하는 네트워크 집합

    <br/>

    * IGP(Internal Gateway Protocol)

        * 자율 시스템(Autonomous System) 내에서 사용하는 라우팅 프로토콜

        * RIP, IGRP, EIGRP, OSPF, IS-IS

    * EGP(Exterior Gateway Protocol)

        * 자율 시스템(Autonomous System) 간 통신에 사용하는 라우팅 프로토콜
        
        * BGP

## 라우팅 알고리즘

* 다이렉트 커넥티드 라우팅(Direct Connected Routing)

    * 설정된 IP 주소와 서브넷 마스크 정보를 통해 자동적으로 계산된(형성된) 경로를 의미

<br/>

* 스태틱 라우팅(Non Adaptive Routing / Fixed Routing)

    * 네트워크 관리자가 수동으로 경로 지정

<br/>

* 다이나믹 라우팅(Adaptive Routing)

    * 라우터 간 연결 정보를 교환하여 경로를 학습

    * 네트워크 토폴로지와 트래픽 변화에 따라 홉 카운트, 거리, 전송시간 지표가 달라지고 이를 반영하여 경로가 변경됨

    * 다이나믹 라우팅 유형

        * 고립형(Isolated)
        
        * 중심형(Centralized) 

        * 분산형(Distributed)

## 경로 결정 방식

* 디스턴스 벡터

    * 벨만 포드 알고리즘 사용

    * 일정 주기로 경로 갱신

    * Distance Vector는 경로를 결정할 때 통과해야 하는 라우터의 수가 적은 쪽으로 경로를 결정함

    * 네트워크 변화 발생 시 해당 정보를 인접한 라우터에 정기적으로 전달 하고, 인접 라우터에서는 라우팅 테이블에 정보갱신

    * 라우팅 정보를 모든 라우터에 주기적으로 갱신하므로 망 자체의 트래픽 부담이 큼

    * RIP, IGRP, EIGRP / BGP

* 링크 스테이트

    * 다익스트라 알고리즘 사용

    * 변경 발생시 갱신

    * Link State는 네트워크 대역폭, 지연 정보 등을 종합적으로 고려해 Cost를 산정하고 해당 Link의 Cost에 따라 경로를 결정함

    * 라우터와 라우터를 연결하는 Link 상태에 따라 최적의 경로 설정

    * 네트워크 전체 정보 유지를 위한 많은 메모리 소요

    * OSPF, IS-IS

## Links

* [라우팅 프로토콜](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=nackji80&logNo=221431942767): Distance Vector Routing vs Link State Routing
