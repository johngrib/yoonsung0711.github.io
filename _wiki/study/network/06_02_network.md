---
layout  : wiki
title   : 방화벽 (3계층 장비)
summary : 
date    : 2021-11-06 09:00:00 +0900
updated : 2021-11-06 09:00:01 +0900
tag     : network
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![firewall1](https://user-images.githubusercontent.com/24386223/140931151-fe488163-3943-4036-88be-525785f55e3d.png)

_↑↑↑ 방화벽_

## 개 요

* 패킷 필터링을 위해 사용하는 네트워크 장치

## 방화벽을 사용하는 이유

* 접근 통제 / 인가 (Firewall)

    * 인가된 트래픽만 허용하고 그 외의 접근은 통제하는 방식

    * 게이트웨이 역할을 하므로 트래픽이 집중됨

    * 다수의 방화벽 장치를 묶고 양 단에 로드밸런서를 설치하기도 함


## 방화벽의 종류와 작동 방식

* 일반 방화벽

    * 3 계층 방화벽

        * 패킷 필터링 방화벽 (Stateless Firewall)

        * 5 튜플(Source IP, Destination IP, Protocol No, Source Port, Destination Port)의 정보에 기반하여 일치된 패킷을 허용 또는 차단

            |출발지|목적지|서비스 포트|접근|
            |:--:|:--:|:--:|:--:|
            |10.10|20.20|443|허용|
            |Any|Any|Any|불가|

        * Protocol No는 Tcp, Udp를 구분하며 ‘0’ 값은 TCP, ‘1’ 값은 UDP임

    * 4 계층 방화벽

        * 세션 상태에 기반(Stateful Packet Inspection)하여 트래픽을 허용 / 통제하는 방화벽
        
        * 5 튜플 외에도 3, 4 계층의 세부적인 필드도 함께 확인하여, 세션 탈취 공격을 방어함


* 웹 애플리케이션 방화벽(Web Application Firewall)

    * 7 계층 방화벽: 애플리케이션 프로토콜과 패킷 내용에 기반하여 트래픽의 허용 / 통제를 결정하는 방화벽

## 4계층의 세션 관리 이슈


## 방화벽의 한계

* 시스템, 계정 탈취 공격은 방지 가능

* 서비스 거부 공격(DDOS, Worms), 백도어, 애플리케이션의 알려진 취약점 등을 이용한 공격에는 대응 불가

## 방화벽을 보완하는 보안 장치

* 침입 탐지 시스템(Instruction Detection System)

    * 네트워크에서 발생하는 이벤트를 모니터링하여 보안 정책에 대한 시급한 위협에 관한 징후를 분석하는 프로세스

    * 탐지 방식

        * 호스트 기반 (Host-based)

        * 네트워크 기반 (Network-based)

* 침입 방지 시스템(Intruction Prevention System)

    * 탐지된 위협에 대한 네트워크 연결을 적극적으로 막는 프로세스

* 차세대 침입 방지 시스템(Next Generation IPS)

    * 애플리케이션 인지

    * 지능형 지속 공격(Advanced Persistent Threat) 공격 방어

<br/>

## Links

* [neocjmix.github.io](https://neocjmix.github.io/etc/2015/08/26/add-server): 서버 추가를 통한 성능개선
