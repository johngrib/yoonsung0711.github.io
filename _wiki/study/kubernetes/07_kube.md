---
layout  : wiki
title   : 외부와의 연결 방법을 다양한 방식으로 제공하는 쿠버네티스 오브젝트들 (노드포트/ 인그레스/로드밸런서)
summary : 디플로이먼트로 쿠버네티스 파드 관리하기
date    : 2021-10-10 09:00:00 +0900
updated : 2021-10-10 09:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![nodeport](https://user-images.githubusercontent.com/65143458/138709678-c4d2258c-36d2-4e69-bc16-f19ba1181a03.png)

_↑↑↑ 노드포트_


![kubernetes service](https://user-images.githubusercontent.com/65143458/138579431-94a0dbfd-6184-4e7c-ab39-2f5ecb056089.png)


_↑↑↑ 인그레스_


![load balancer](https://user-images.githubusercontent.com/65143458/138710182-82cde59d-4e96-4968-bba0-9e51951e0dc8.png)


_↑↑↑ 로드밸런서_


<br/>

## 개 요

* 쿠버네티스 클러스터는 기본적으로 폐쇄된 네트워크임

* 파드에 배포한 애플리케이션을 외부와 연결하기 위해서는 네트워크 연결을 돕는 서비스 타입의 쿠버네티스 오브젝트를 생성해야 함

* 서비스 타입으로는 노드포트(노드의 포트를 개방하고, 포트에 수신된 요청을 서비스로 전달), 인그레스 (도메인 주소를 노출하고, 해당 도메인 주소로 수신된 요청을 미리 매핑해둔 서비스[노트포트/로드밸런서]로 전달), 로드밸런서 (외부에 ip주소를 노출하고, 해당 주소로 수신된 요청을 파드에 분산하여 전달)


<br/>

## 디플로이먼트 - 서비스 연결

* 스펙 파일(yaml)로 연결하기

    * 먼저 디플로이먼트로 파드에 배포함


        ```shell
        $ kubectl create deployment <디플로이먼트명> --image=<이미지명>
        ```

    * 노드포트 spec에 디플로이먼트명을 selector로 명시하여 배포


        ```shell
        apiVersion: v1
        kind: Service
        metadata:
            name: np-svc
        spec:
            selector:
                app: np-pods
        ports:
            - name: http
              protocol: TCP
              port: 80
              targetPort: 80
              nodePort: 30000
        type: NodePort% 
        ```

* expose (kubectl 명령)로 연결하기

    * 디플로이먼트와 서비스 연결을 한번에 해결함

        ```shell
        $ kubectl expose deployment <디플로이먼트명> --type=NodePort --name=<서비스명>
        ```

## 인그레스 컨트롤러 설치 - 인그레스 설정 적용 - 인그레스 연결

* 인그레스 컨트롤러 설치


    ```shell
    # 설치시 아래와 같은 오브젝트들이 생성됨 
    namespace/ingress-nginx created
    configmap/nginx-configuration created
    configmap/tcp-services created
    configmap/udp-services created
    serviceaccount/nginx-ingress-serviceaccount created
    ...
    ...
    limitrange/ingress-nginx created
    ```

* 인그레스 설정 적용

    ```shell
    ```

* 인그레스 연결

    ```shell
    apiversion: v1
    kind: service
    metadata:
        name: nginx-ingress-controller
        namespace: ingress-nginx
    spec:
        ports:
        - name: http
          protocol: tcp
          port: 80
          targetport: 80
          nodeport: 30100
        - name: https
          protocol: tcp
          port: 443
          targetport: 443
          nodeport: 30101
        selector:
            app.kubernetes.io/name: ingress-nginx
        type: nodeport
    ```




