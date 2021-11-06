---
layout  : wiki
title   : 데몬셋, 레플리카셋, 스테이트풀셋, 크론잡
summary : 쿠버네티스 컨트롤러 오브젝트
date    : 2021-11-1 09:00:00 +0900
updated : 2021-11-1 09:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}


![k8s-arch](https://user-images.githubusercontent.com/65143458/140595666-e9337d6a-e49b-42a2-81c9-f82490941282.png)

_↑↑↑ 쿠버네티스 아키텍처_

<img width="1061" alt="kube_controllers" src="https://user-images.githubusercontent.com/65143458/140595639-1bfa1b35-055c-4579-9dd6-5807058ae193.png">

_↑↑↑ 쿠버네티스 컨트롤러_


<br/>

## 개 요

* 클러스터 파드의 배포, 복제본의 개수 등을 관리하는 컨트롤러 객체들

## 데몬셋

* 모든 노드 또는 특정 노드 위에 단일 파드를 지속적으로 실행시키기 위한 쿠버네티스 객체

* 유스케이스 

    * 파드 단위가 아닌 노드 단위의 로그 수집이 필요한 경우

    * 노드의 모니터링을 위한 데몬을 실행할 경우 

    * 분산 컴퓨팅 클러스터 개체 스토리지를 노드 단위로 구성할 필요가 있는 경우

* 명세 예시 

    ```yaml
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: fluentd-elasticsearch
      namespace: kube-system
      labels:
        k8s-app: fluentd-logging
    spec:
      selector:
        matchLabels:
          name: fluentd-elasticsearch
      template:
        metadata:
          labels:
            name: fluentd-elasticsearch
        spec:
          tolerations:
          - key: node-role.kubernetes.io/master
            effect: NoSchedule
          containers:
          - name: fluentd-elasticsearch
            image: k8s.gcr.io/fluentd-elasticsearch:1.20
            resources:
              limits:
                memory: 200Mi
              requests:
                cpu: 100m
                memory: 200Mi
            volumeMounts:
            - name: varlog
              mountPath: /var/log
            - name: varlibdockercontainers
              mountPath: /var/lib/docker/containers
              readOnly: true
          terminationGracePeriodSeconds: 30
          volumes:
          - name: varlog
            hostPath:
              path: /var/log
          - name: varlibdockercontainers
            hostPath:
              path: /var/lib/docker/containers

    ```

## 레플리카셋

* 파드 위에 지정한 개수의 복제본이 실행되는 것을 보장하는 컨트롤러임

* 디플로이먼트는 레플리카셋을 관리할 뿐만 아니라 다른 유용한 기능을 제공하므로 레플리카셋 보다는 디플로이먼틀을 직접 사용하는 것이 권장됨

* 유스케이스 
    
    * 서버 과부하, 장애 등와 같은 서비스 불능 상황 발생시 자동으로 서버를 복제하여 파드의 동작을 보장함

    * Replication Set 기반의 디스크 볼륨 관리는 하나의 컨트롤러로 여러 Pod에 대해 디스크를 각각 지정해서 관리할 수 없는 문제가 있음

* 명세 예시

    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: nginx-replicaset
    spec:
      replicas: 3
      selector:
        matchLabels: 
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx-container
            image: nginx
            ports:
            - containerPort: 80
    ```

## 스테이트풀셋

* 상태가 있는 파드들을 관리하기 위한 컨트롤러로써, 개별 Pod에 대한 디스크 볼륨 관리를 지원한다.

* 유스케이스

    * 볼륨에 접속해야 하는 파드(리디스)가 재배포 되거나 재시작되더라도 같은 볼륨에 접속할 수 있도록 해야 하는 경우

    * 웹앱이 미리 정의한 네트워크 식별자를 이용해 자신의 복제본과 통신해야 하는 경우

    * 파드의 실행 순서와 스케일링을 조절해야 하는 경우

* 명세 예시

    ```yaml
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
      labels:
        app: nginx
    spec:
      serviceName: "nginx"
      selector:
        matchLabels:
          app: nginx
      replicas: 14
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
      volumeClaimTemplates:
      - metadata:
          name: www
        spec:
          accessModes: [ "ReadWriteOnce" ]
          resources:
            requests:
              storage: 1Gi
          storageClassName: thin-disk
    ```


## 크론잡

* 비동기 배치 작업을 수행하기 위한 컨트롤러로서, 크론잡을 이용해 애플리케이션을 원하는 시점에 실행할 수 있음

* 미리 정의된 작업 단위를 실행한 후 컨테이너를 종료함

* 유스케이스

    * 회원제로 운영되는 서비스 운영시, 멤버십 유효기간이 지난 회원들을 비활성화 시키거나 계정을 삭제해야 하는 경우

    * 특정 주기로 반복되는 뉴스 레터, 이메일을 발송해야 하는 경우

    * 캐시 데이터를 주기적으로 삭제해야 하는 경우

    * 웹사이트의 정적 자원 링크의 연결이 끊어졌는지를 정기적으 체크할 필요가 있는 경우

    * 비디오 인코딩, 대량 이메일 발송 등과 같이 실행에 오랜 시간이 소요되는 작업을 처리하는 경우

* 명세 예시

    ```yaml
    apiVersion: batch/v1beta
    kind: CronJob
    metadata:
        name: random-generator
    spec:
        schedule: "*/3 * * * *"
        jobTemplate:
            spec:
                containers:
                - images: k8spatterns/random-generator:1.0
                  name: random-generator
                  command: [ "java", "-cp", "/", "RandomRunner", "/numbers.txt", "10000"]
                restartPolicy: OnFailure
    ```

## Links




