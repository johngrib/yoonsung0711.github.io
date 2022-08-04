---
layout  : wiki
title   : DNS (Domain Name System)
summary : 도메인 주소를 IP 주소로 변환
date    : 2021-11-13 10:00:00 +0900
updated : 2021-11-13 10:00:01 +0900
tag     : network
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![dns](https://user-images.githubusercontent.com/65143458/140613151-df91e03c-0727-417d-950d-d443f0dd828a.png)

_↑↑↑ DNS Contept_

![recursive_query](https://user-images.githubusercontent.com/65143458/140613224-0541862a-9fdf-44c0-9bf8-ef6be52c4ad3.gif)

_↑↑↑ DNS Recursive Query_

## 개 요

* 연락처 목록, 전화번호부가 전화번호로의 손쉬운 연결을 돕는 것과 같이 도메인 주소를 IP 주소로 쉽게 변환하도록 돕는 전송 제어 프로토콜(Transmission Control Protocol)

<br/>

## DNS의 구조와 명명 규칙

* 도메인 계층

    * 루트 도메인, Top-Level 도메인, Second-Level 도메인, Third-Level 도메인

    * 알파벳, 숫자, "-"만 사용할 수 있으며 대소문자 구분이 없음

<br/>

## DNS 동작 방식

* 도메인 쿼리

    * 로컬에서 도메인을 IP 주소 정보로 직접 변환하는 hosts 파일 사용

    * DNS 서버에 쿼리를 하기 전 로컬에 있는 DNS 캐시 정보를 먼저 확인

<br/>

## DNS의 주요 레코드

* A Record

    * 도메인 이름에 IP 주소를 매핑

    * google.co.kr -> 172.217.25.67

        ```shell
        $ nslookup google.co.kr
        ```

* CNAME

    * 도메인 이름의 별칭

    * www.google.co.kr -> google.co.kr

* AAAA Record

* TXT Record

    ```shell
    yoonsung0711$ nslookup
    > set q=txt
    > daum.net
    Non-authoritative answer:
    daum.net	text = "google-site-verification=CQHqDeJv5QbFRN_ViQvKJa3jeMDrCiw2iEPs1XcdAmk"
    daum.net	text = "v=spf1 include:_spf.daum.net ~all"
    daum.net	text = "google-site-verification=0w8tAk4ZN8tACAfxeAkgBQy3MfsZiFD3gt5zCeouVNQ"
    ```

* SOA(Start of Authority) Record

    ```shell
    # /etc/named.conf <- 주 환경설정 파일
    # /etc/named.rfc1912.zones <- 도메인 등록 파일
    # /var/named/chroot/var/named/test.co.kr.zone <-도메인 정보 파일
    ``` 
* NS(Name Server) Record

* MX Record

    ```shell
    yoonsung0711$ nslookup
    > set q=mx
    > daum.net
    Non-authoritative answer:
    daum.net	mail exchanger = 10 mx2.hanmail.net.
    daum.net	mail exchanger = 10 mx4.hanmail.net.
    daum.net	mail exchanger = 10 mx1.hanmail.net.
    daum.net	mail exchanger = 10 mx3.hanmail.net.
    ```

* PTR (Pointer) 레코드

    ```shell
    yoonsung0711$ nslookup
    > set q=PTR
    > 209.132.183.181
    Non-authoritative answer:
    181.183.132.209.in-addr.arpa	name = origin-www2.redhat.com.
    ```

## Links

