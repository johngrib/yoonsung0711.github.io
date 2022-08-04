---
layout  : wiki
title   : 컨테이너 이미지 만들기
summary : Dockerfile로 빌드하기
date    : 2021-11-21 9:00:00 +0900
updated : 2021-11-21 9:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![dockerfile_build](https://user-images.githubusercontent.com/65143458/142753137-33ccb06b-7383-4b23-aa11-45b226851c82.png)

_↑↑↑ 도커파일 이미지 빌드_

<br/>

## 개 요

* 도커파일은 도커 이미지를 생성하기 위해 필요한 명령어들을 DSL 형태로 정의한 텍스트 파일임

<br/>

## 단계적으로 이미지 빌드 작업 향상하기

* 호스트 컴퓨터에서 빌드하고 artifact 복사하기

    ```dockerfile
    FROM openjdk:8
    EXPOSE 60431
    COPY ./target/app.jar /opt/app.jar
    WORDIR /opt
    ENTRYPOINT [ "java", "-jar", "app.jar" ]
    ```

    | Docker                                              |      Bash                                       |
    |-----------------------------------------------------|:----------------------------------------------- |
    | FROM openjdk:8                                      | apt-get install openjdk-8-jdk                   |
    | EXPOSE 60431                                        | export EXPOSE=60431                             |
    | COPY ./app.jar /opt/app.jar                         | cp ./opt                                        |
    | WORKDIR /opt                                        | cd ./opt                                        |
    | ENTRYPOINT [ "java", "-jar", "app.jar"]             | ./java -jar app.jar                             |

* 배포에 최적화된 이미지를 기본 이미지로 사용하기

    ```dockerfile
    # "Distroless" images contain only your application and its runtime dependencies. 
    # They do not contain package managers, shells or any other programs you would expect to find in a standard Linux distribution.


    FROM gcr.io/distroless/java:8
    ```

* 컨테이너에서 소스코드 빌드하기

    ```dockerfile
    # Dockerfile
    FROM openjdk:8
    WORKDIR /app
    COPY src src
    COPY build.gradle.kts settings.kts gradlew gradlew.bat ./
    COPY gradle gradle
    RUN ./gradlew --no-daemon --exclude-task test build
    EXPOSE 60433
    ```

* 멀티 스테이지 빌드로 최적화 하기

    ```dockerfile
    # Dockerfile
    FROM openjdk:8 AS base
    WORKDIR /app
    COPY build.gradle.kts settings.kts gradlew gradlew.bat ./
    COPY gradle gradle
    
    FROM base as source
    WORKDIR /app
    COPY src src

    FROM source AS build
    WORKDIR /app
    RUN ./gradlew --no-daemon --exclude-task test build

    FROM gcr.io/distroless/java:8 AS release
    WORKDIR /app
    EXPOSE 60434
    ENTRYPOINT ["java", "-jar", "app.jar"]

    ```

## 이미지/컨테이너 정리하기

* dangling 이미지 정리하기

    ```shell
    $ docker rmi $(docker images -f dangling=true -q)

    또는 

    $ docker image prune
    ```

* 모든 이미지 삭제하기

    ```shell
    $ docker rm -vf $(docker ps -a -q)
    ```

* 모든 컨테이너 삭제하기 

    ```shell
    $ docker rmi -f $(docker images -a -q)
    ```
