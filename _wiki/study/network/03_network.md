---
layout  : wiki
title   : 네트워크 통신 기초
summary : 
date    : 2021-10-27 09:00:00 +0900
updated : 2021-10-27 09:00:01 +0900
tag     : network
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![arp](https://user-images.githubusercontent.com/65143458/139087786-f8baec36-bda6-49df-9bfb-54527aafe2e4.png)

_↑↑↑ arp_

![subnetting](https://user-images.githubusercontent.com/65143458/139087302-2abe52f0-2356-4503-a939-03daa91cb990.png)

_↑↑↑ subnetting_


## 개 요

* 

## ARP (Address Resolution Protocol)

* 2계층(데이터 링크 계층)의 물리적 주소인 MAC 주소와 3계층(네트워크 계층)의 논리적 주소인 IP를 연결하기 위한 메커니즘 및 통신 방식

    
## 스위치의 동작


* 플러딩

    * 스위치는 자신이 관리하는 MAC 주소 테이블에서 패킷 수신자의 MAC 주소를 확인할 수 없을 때 패킷이 들어온 포트를 제외한 모든 포트로 패킷을 전달함(유니캐스트 플러딩 또는 언노운 유니케스트)

    * 스위치의 유니캐스트 플러딩을 유도해 주변 통신을 모니터링하는 공격 방식을 MAC 플러딩이라고 함

    * MAC 주소 테이블에 MAC 주소와 포트 번호를 짝지워 기록하는 것을 어드레스 러닝이라고 함

    * 어드레스 러닝 과정을 통해 학습한 주소 정보로 수신된 패킷의 선택적 전송 (포워딩 / 필터링)을 함


## 스위치가 사용하는 가상화 기술

* VLAN

    * 논리적인 Network 분리 기술 (라우터는 물리적으로 Network를 분리함)

    * 숫자로 Network 이름을 구분함

* STP

    * 



