---
layout  : wiki
title   : DNS (Domain Name System)
summary : 
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

