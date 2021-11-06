---
layout  : wiki
title   : Vagrant
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

## 개 요

gitlab-backup create

gitlab-rake gitlab:backup:create

/etc/gitlab/gitlab-secrets.json
/etc/gitlab/gitlab.rb

기본설정으로 세팅되어 있다면, /var/opt/gitlab/backups 경로에 타임스템프값_버전_gitlab_backup.tar 와 같은 형태로 백업파일이 생성된다.


## links

[gitlab 컨테이너 backup/restore(백업 및 복원)](https://ykarma1996.tistory.com/111)

[gitlab 백업 및 복구](https://www.lesstif.com/gitbook/gitlab-19857653.html)

[[vagrant] 공유폴더 (Synced Folders) 만들기](https://m.blog.naver.com/sory1008/220755126599)

[vagrant 설정](https://dolhani.tistory.com/560)
