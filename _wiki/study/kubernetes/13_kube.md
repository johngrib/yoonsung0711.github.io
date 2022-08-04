---
layout  : wiki
title   : 멀티 스테이지 빌드로 도커 컨테이너를 만드는 방법
summary : Dockerfile과 멀티 스테이지 빌드 
date    : 2021-11-21 10:00:00 +0900
updated : 2021-11-21 10:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![docker-layers2](https://user-images.githubusercontent.com/24386223/141614388-379d382c-926d-4877-9e23-5474d67dc474.png)

_↑↑↑ 레이어드 이미지1_

<br/>

![layered_image](https://user-images.githubusercontent.com/24386223/141614356-31b1f73a-c57f-41a3-a13f-7dee840987ef.png)

_↑↑↑ 레이어드 이미지2_

<br/>

## 개 요

* 컨테이너는 여러 층의 layer 이미지를 쌓아 올리는 방식으로 생성되는 데, 빌드/테스트/배포와 같은 중요 관심 사에 따라 필요한 필요한 최소한의 레이어만 사용하도록 단계를 나누는 방법

<br/>

## 도커 멀티 스테이지 이미지 빌드

* 레이어드 이미지 만들기

    ```dockerfile
    FROM node:16-alpine AS base

    ... (작업 1)
    ... (작업 2)

    FROM base AS build

    ... (작업 3)
    ... (작업 4)

    FROM base AS test

    ... (작업 5)
    ... (작업 6)

    FROM node: 16-alpine AS release

    ... (작업 7)
    ... (작업 8)
    ```

* 이미지 빌드 환경변수 주입하기

    ```dockerfile
    # Dockerfile
    ARG node_env

    # CLI
    $ docker run --it -p 3000:3000 $(docker build --build-arg build_env='development' -q --no-cache .)
    ```

* 컨테이너 런타임 환경변수 주입하기

    ```dockerfile
    # Dockerfile
    ENV NODE_ENV='production'
    ```

* 작업 폴더 설정하기

    ```dockerfile
    # Dockerfile
    WORKDIR /app
    ```

* 파일 복사하기

    ```dockerfile
    # Dockerfile
    COPY src src
    ```

* cli 명령어 실행하기

    ```dockerfile
    # Dockerfile
    RUN npm ci 
    ```

## 컨테이너 시작/접속/실행/종료/삭제

* 컨테이너 시작

    ```
    $ docker run --it --name <컨테이너이름> <이미지이름>:<버전>
    ```

* 컨테이너 접속

    ```
    $ docker run -d --name <컨테이너이름> <이미지이름>:<버전>
    $ docker attach <컨테이너이름>

    ```

* 컨테이너 실행

    ```
    $ docker run -d --name <컨테이너이름> <이미지이름>:<버전>
    $ docker attach <컨테이너이름>

    ```

* 컨테이너 종료/삭제


    ```
    $ docker stop <컨테이너이름>
    $ docker rm <컨테이너이름>

    ```
