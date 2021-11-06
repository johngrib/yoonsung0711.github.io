---
layout  : wiki
title   : PV (PersistentVolume), PVC (PersistentVolumeClaim)
summary : 쿠버네티스 볼륨 오브젝트
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

<br/>

## 개 요

* 파드(컨테이너)는 라이프사이클이 일시적(ephermeral)이므로 파드에서 실행되는 상태 비저장(stateless) 애플리케이션의 상태를 영속적으로 저장할 수 있는 방법이 필요함

* 쿠버네티스는 컨테이너가 아닌 파드에 종속되는 데이터 저장소를 생성(PV)하고 이를 필요한 만큼 파드(여러 컨테이너)에 할당(PVC)하는 객체를 이용해 클러스터의 영속성을 유지함

<br/>

## PV (PersistentVolume) / PVC (PersistentVolumeClaim)

* 클러스터가 데이터를 저장하기 위한 저장소(PV)와 저장소를 이용(PVC)하는 객체

* 파드가 아닌 PV 객체에 데이터가 저장되기 때문에, 파드가 대체되어도 데이터가 유지됨


* 유스케이스 
    
    * 애플리케이션에서 사용될 데이터베이스 인스턴스를 PV로 생성하여 사용함

* 명세 예시

    ```yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    metadata: 
        ...(중략)...
        uid: .....
    data:
      prometheus.yml: |-
        global:
          scrape_interval:     15s
          evaluation_interval: 15s
        scrape_configs:
          - job_name: 'prometheus'
            static_configs:
            - targets: ['localhost:9090']
          - job_name: 'example'
            static_configs:
            - targets: ['www.example.com:9100']
    ---
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: prometheus-pvc
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 20Gi
    ---
    kind: Deployment
    metadata:
      name: prometheus
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: prometheus
      template:
        metadata:
          labels: 
            app: prometheus
        spec:
          containers:
          - name: prometheus
            image: prom/prometheus
            volumeMounts:
            - name: config-volume
              mountPath: /etc/prometheus
            - name: data
              mountPath: /prometheus
            ports:
            - containerPort: 80
          securityContext:
            runAsUser: 0
          volumes:
            - name: config-volume
              configMap:
                name: prometheus-cm
            - name: data
              persistentVolumeClaim:
                claimName: prometheus-pvc
    ```

## Links


[Medium](https://ystatit.medium.com/k8s-pv-pvc-and-configmap-for-prometheus-and-grafana-caa044b0d82b): K8S — PV, PVC and ConfigMap for Prometheus and Grafana



