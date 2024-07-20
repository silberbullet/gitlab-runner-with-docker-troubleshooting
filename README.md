# gitlab-runner-with-docker-troubleshooting

#### 배경

> TO-BE 사내 CRM 프로젝트를 진행하면서 `DevOps 영역`까지 맡게 되었다.
> AS-IS 사내 CRM 레거시 프로젝트는 `SVN` 형상관리와 `Jenkins` 빌드 도구로 배포를 관리하고 있었다. 개발계는 CentOS Linux 환경에서 `수동 배포`가 이루어지고 있었다.</br>
> TO-BE에서는 Ubuntu 운영체제를 시작으로 사내망을 구성하기 위해 `Docker`를 이용하여 `GitLab` 서버를 뛰워 형상관리를 시작하였다.</br>
> 그 후 `GitLab-Runner`와 `SSH`을 이용하여 개발계를 **자동 빌드, 배포화 시키는 CI/CD 환경**을 구축하였다.</br>

#### 목표

> 수동으로 이루어져 매번 빌드 요청을 해야만 했던 프로세스를 이제는 자동화로 이어져 관리자가 편리함을 느끼게 하자. </br>
> SI 회사 이다 보니 CRM DevOps 담당자가 없기 때문에 메뉴얼 가이드를 작성하자.</br>
> 사내 인원들에게 DevOps를 리뷰하여 `배포`를 쉽게 접근 할 수 있도록 해보자.</br>

## 목차

1. [개발계 구성](#-개발계_구성)
2. [배포 전략](#-배포_전략)
3. [TroubleShooting](#-TroubleShooting)

---

## ▶ 개발계 구성

| 항목구분                     | 세부항목         | 설명                                                                                                           |
| ---------------------------- | ---------------- | -------------------------------------------------------------------------------------------------------------- |
| 192.168.0.xxx<br>(GitLab 용) | OS               | Ubuntu 22.04.3 LTS, 계정 , RAM 16g                                                                             |
|                              | SSH Service      | SSH SERVICE 사용, ssh –p 22 root@192.168.0.xxx 접속 가능                                                       |
|                              | Docker Engine    | Docker engine 설치 완료, Docker Bridge 네트워크 활성화 ( 172.17.0.1 )                                          |
|                              | Docker Image     | `gitlab/gitlab-ce` (3.01GB), `gitlab/gitlab-runner` (766MB), node (1.11GB), `gradle` (715MB), `docker` (335MB) |
|                              | Docker Container | `gitlab` (:5080 ), `vue-runner`, `middle-runner`, `sv-runner`                                                  |
|                              | 포트포워딩       | 유무<br>:80 -> :5080 Redirect 설정                                                                             |

| 항목구분                  | 세부항목         | 설명                                                                            |
| ------------------------- | ---------------- | ------------------------------------------------------------------------------- |
| 192.168.0.xxx<br>(CRM 용) | OS               | Ubuntu 22.04.3 LTS, RAM 16g                                                     |
|                           | SSH Service      | SSH SERVICE 사용, ssh –p 22 root@192.168.0.xxx 접속 가능                        |
|                           | Docker Engine    | Docker engine 설치 완료, Docker Bridge 네트워크 활성화 ( 172.17.0.1 )           |
|                           | Docker Image     | `crmvue` (240MB), `crmmiddleware` (446MB), `crmserver` (452MB), `redis` (117MB) |
|                           | Docker Container | `vue` (:9070 ), `middleware` (:9080 ), `server` (:9000 ), `redis` (:6379 )      |
|                           | 포트포워딩       | 없음                                                                            |

> **참고**: 192.168.0.119 : 5080 포트번호 실행되고 있는 GitLab 서버를 일반 도메인 주소로 접속이 가능하기 위해 포트포워딩 설정</br> > **GitLab 서버와 개발계 CRM 서버를 분리한 이유**

## ▶ 배포 전략

1. **CI & Image Push**

2. **Image Pull & CD**

## ▶ TroubleShooting
