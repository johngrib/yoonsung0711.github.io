---
layout  : wiki
title   : Kubernetes 클러스터 구성요소와 기본 개념 살펴보기
summary : 아키텍처와 인스턴스 라이프사이클
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

![components-of-kubernetes](https://user-images.githubusercontent.com/24386223/136684906-d60486f9-4a45-4340-a2d4-7e2c073ab0ea.jpg)

## 개 요

* 구성 요소 (Components of Kubernetes)

    * 컴포넌트 리스트

        ```
        $ kubectl get pods --all-namespaces

        NAMESPACE     NAME                                      READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
        kube-system   calico-kube-controllers-99c9b6f64-5cvw6   1/1     Running   1          22h   172.16.171.70   m-k8s    <none>           <none>
        kube-system   calico-node-6r9bf                         1/1     Running   1          22h   192.168.1.103   w3-k8s   <none>           <none>
        kube-system   calico-node-jd68n                         1/1     Running   1          22h   192.168.1.102   w2-k8s   <none>           <none>
        kube-system   calico-node-n6ccx                         1/1     Running   1          22h   192.168.1.10    m-k8s    <none>           <none>
        kube-system   calico-node-nlf7r                         1/1     Running   1          22h   192.168.1.101   w1-k8s   <none>           <none>
        kube-system   coredns-66bff467f8-4j4lj                  1/1     Running   1          22h   172.16.171.69   m-k8s    <none>           <none>
        kube-system   coredns-66bff467f8-n76vp                  1/1     Running   1          22h   172.16.171.68   m-k8s    <none>           <none>
        kube-system   etcd-m-k8s                                1/1     Running   1          22h   192.168.1.10    m-k8s    <none>           <none>
        kube-system   kube-apiserver-m-k8s                      1/1     Running   1          22h   192.168.1.10    m-k8s    <none>           <none>
        kube-system   kube-controller-manager-m-k8s             1/1     Running   1          22h   192.168.1.10    m-k8s    <none>           <none>
        kube-system   kube-proxy-2tpvj                          1/1     Running   1          22h   192.168.1.10    m-k8s    <none>           <none>
        kube-system   kube-proxy-gmqv6                          1/1     Running   1          22h   192.168.1.102   w2-k8s   <none>           <none>
        kube-system   kube-proxy-kzhpv                          1/1     Running   1          22h   192.168.1.101   w1-k8s   <none>           <none>
        kube-system   kube-proxy-s6p67                          1/1     Running   1          22h   192.168.1.103   w3-k8s   <none>           <none>
        kube-system   kube-scheduler-m-k8s                      1/1     Running   1          22h   192.168.1.10    m-k8s    <none>           <none>
        ```


    * 마스터 노드

        * 설정 저장소(etc distributed): etcd-m-k8s

        * 스케줄러(scheduler): kube-scheduler-m-k8s

            * api-server의 이벤트를 수신하여 Pod에 적합한 노드를 할당함

            * Pod 할당 내역을 etcd에 갱신함

        * 컨트롤러 매니저(controller manager): 여러 컨트롤러(Node/Route/Service) 기능이 하나로 통합되어 있으며, 스케쥴러를 참고하여 정확한 수의 pod가 실행되도록 함

        * API 서버(api-server): kube-apiserver-m-k8s

            * 쿠버네티스 클러스터를 관리, 생성, 구성하는 데 필요한 인터페이스를 제공하는 서버

            * kubectl 등으로부터 요청을 수신하여 etcd 등에 Pod 정보 갱신

        * 컨테이너 네트워크 인터페이스(CNI) / 네트워크 플러그인: calico-* - 가상의 컴퓨터 네트워크(Overlay)로 다음과 같은 역할 수행

            * 단일 Host 내에 존재하는 Pod 간의 통신

            * 서로 다른 Host 내에 존재하는 Pod 간의 통신

            * Pod 과 다른 AWS 서비스 간의 통신

            * Pod 과 온프레미스 데이터 센터 간의 통신

            * Pod 과 인터넷 간의 통신

        * 프록시: kube-proxy-*

            * 각 노드에서 실행되어 UDP, TCP, SCTP를 전달함 

            * 로드 밸런싱 수행

        * 네임서버(coreDNS): coredns-*

            * 쿠버네티스 클러스터의 DNS 역할(도메인과 Service나 Pod의 IP를 매핑) 수행


    * 워커 노드

        * 컨테이너 실행 환경 (docker, containerd, crio-o, runc)

        * 에이전트 (kubelet)

            * 각 노드에서 실행되는 에이전트

            * 파드에서 컨테이너가 확실하게 동작하도록 관리함

        * 프록시: kube-proxy-*

            * 각 노드에서 실행되어 UDP, TCP, SCTP를 전달함 

            * 로드 밸런싱 수행

## vm 접속하기

* 접속 방법

    * 실습환경에서 vm 접속시 ssh-keygen + ssh-copy-id와 /etc/hosts 정의를 이용하면 리모트 vm 접속을 보다 편리하게 할 수 있는 장점이 있음

    * 패스워드 방식으로 ssh 접속을 한다면  아래와 같이 sshpass와 쉘 함수를 정의하여 리모트 vm 접속을 간편하게 만들 수 있음

* 구성 방법 (sshpass + ssh + 쉘 함수)

    * sshpass 설치

        ```shell
        # 첫번째 방법
        $ curl -L https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb > sshpass.rb && brew install sshpass.rb && rm sshpass.rb

        # 두번째 방법
        $ brew install hudochenkov/sshpass/sshpass
        ```

    * ~/.bash_profile (~/.zshrc) 등에 프로시져 정의하기

        ```shell
        
        kmaster() {
            sshpass -p'vagrant' ssh root@127.0.0.1 -p60010
        }

        kworker1() {
            sshpass -p'vagrant' ssh root@127.0.0.1 -p60101
        }

        kworker2() {
            sshpass -p'vagrant' ssh root@127.0.0.1 -p60102
        }

        kworker3() {
            sshpass -p'vagrant' ssh root@127.0.0.1 -p60103
        }
        ```

## Links

* [How to install sshpass on Mac?](https://stackoverflow.com/questions/32255660/how-to-install-sshpass-on-mac/62623099):  Stack Overflow

* [Understanding Kubernetes, Part2: The Scheduler](https://www.youtube.com/watch?v=E3ExWruji7g):  Stack Overflow
