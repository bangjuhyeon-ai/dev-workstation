

\# Docker 기반 개발 워크스테이션 구축



\## 1. 프로젝트 소개



본 프로젝트는 Windows 환경에서 Docker와 Git을 활용하여 개발 워크스테이션을 구축하는 실습이다.



Docker를 이용해 웹 서버를 실행하고, 포트 매핑, Bind Mount, Docker Volume 등의 기능을 실습하였다. 또한 Git과 GitHub를 연동하여 프로젝트 파일을 버전 관리하고 원격 저장소에 업로드하였다.



\## 2. 개발 환경



| 구분 | 사용 환경 |

|---|---|

| 운영체제 | Windows 11 |

| 터미널 | Windows PowerShell |

| 컨테이너 플랫폼 | Docker Desktop |

| Docker 버전 | 29.9.3 |

| Git 버전 | 2.56.0.windows.4 |

| 웹 서버 | Nginx (Alpine) |

| 버전 관리 | Git / GitHub |



\## 3. 프로젝트 폴더 구조



```text

dev-workstation/

├── app/

│   └── index.html

├── docs/

├── data/

├── Dockerfile

└── README.md

```



\## 4. Docker 실습



\### 4-1. Docker 설치 및 확인



```powershell

docker --version

docker info

```



Docker 버전과 Docker Engine이 정상적으로 작동하는 것을 확인하였다.



\### 4-2. Docker 기본 명령어



```powershell

docker images

docker ps

docker ps -a

docker logs my-first-container

docker stats --no-stream my-first-container

```



Docker 이미지, 컨테이너, 로그 및 자원 사용량을 확인하였다.



\### 4-3. Dockerfile 작성 및 이미지 빌드



Dockerfile 내용:



```dockerfile

FROM nginx:alpine

COPY app/ /usr/share/nginx/html/

```



이미지 빌드:



```powershell

docker build -t my-first-web .

```



Nginx 기반 Docker 이미지 생성에 성공하였다.



\### 4-4. 컨테이너 실행 및 포트 매핑



```powershell

docker run -d --name my-first-container -p 8080:80 my-first-web

```



웹 브라우저에서 다음 주소로 접속하였다.



http://localhost:8080



웹페이지가 정상적으로 표시되는 것을 확인하였다.



\### 4-5. Bind Mount 실습



```powershell

docker run -d --name bind-web -p 8081:80 -v "${PWD}\\app:/usr/share/nginx/html" nginx:alpine

```



웹 브라우저에서 다음 주소로 접속하였다.



http://localhost:8081



로컬 컴퓨터의 index.html 파일을 수정한 후 브라우저를 새로고침하여 변경 사항이 즉시 반영되는 것을 확인하였다.



\### 4-6. Docker Volume 실습



Volume 생성:



```powershell

docker volume create site-data

```



데이터 저장:



```powershell

docker run --name volume-test -v site-data:/data alpine sh -c "echo 'Docker Volume 데이터 보존 성공!' > /data/test.txt"

```



기존 컨테이너 삭제:



```powershell

docker rm volume-test

```



새로운 컨테이너에서 데이터 확인:



```powershell

docker run --rm -v site-data:/data alpine cat /data/test.txt

```



기존 컨테이너를 삭제한 후에도 데이터가 유지되는 것을 확인하였다.



\## 5. Git 및 GitHub 연동



Git 버전 확인:



```powershell

git --version

```



Git 저장소 생성:



```powershell

git init

```



파일 등록 및 저장:



```powershell

git add .

git commit -m "Initial Docker workstation setup"

```



GitHub 원격 저장소 연결:



```powershell

git branch -M main

git remote add origin https://github.com/bangjuhyeon-ai/dev-workstation.git

git push -u origin main

```



GitHub에 프로젝트 파일이 정상적으로 업로드된 것을 확인하였다.



GitHub 저장소:



https://github.com/bangjuhyeon-ai/dev-workstation



\## 6. 문제 해결 및 트러블슈팅



\### 문제 1. Docker Engine 실행 오류



\- 발생 상황: Docker 명령어 실행 시 Docker Engine에 연결할 수 없었다.

\- 원인: Docker Desktop이 정상적으로 실행되지 않은 상태였다.

\- 해결 방법: WSL 및 Virtual Machine Platform을 설치하고 시스템을 재시작한 후 Docker Desktop을 실행하였다.

\- 결과: Docker Engine이 정상적으로 실행되었다.



\### 문제 2. Dockerfile 파일 인식 오류



\- 발생 상황: Docker 이미지 빌드 과정에서 Dockerfile을 정상적으로 인식하지 못하였다.

\- 원인: 파일이 Dockerfile.txt라는 이름으로 저장되어 있었다.

\- 해결 방법: 파일 이름을 Dockerfile로 변경하고 올바른 Docker 설정 내용을 작성하였다.

\- 결과: Docker 이미지 빌드에 성공하였다.



\### 문제 3. Git 명령어 인식 오류



\- 발생 상황: PowerShell에서 git 명령어를 인식하지 못하였다.

\- 원인: Git 설치 경로가 명령어 실행 환경에 등록되지 않은 상태였다.

\- 해결 방법: Git for Windows를 설치하고 PATH 설정을 적용한 후 PowerShell을 다시 실행하였다.

\- 결과: git --version 명령어가 정상적으로 실행되었다.



\## 7. 실습 결과



\- \[x] Docker 설치 및 버전 확인

\- \[x] Docker Engine 정상 작동 확인

\- \[x] Docker 이미지 생성

\- \[x] Docker 컨테이너 실행

\- \[x] 포트 매핑 및 웹페이지 접속 확인

\- \[x] Bind Mount 실시간 변경 확인

\- \[x] Docker Volume 데이터 영속성 확인

\- \[x] Git 설치 및 초기 설정

\- \[x] Git Commit 진행

\- \[x] GitHub 원격 저장소 업로드



\## 8. 마무리



이번 실습을 통해 Docker를 활용한 웹 서버 구축부터 컨테이너 실행, 데이터 관리까지 개발 환경의 기본적인 구성 과정을 경험하였다.



특히 Bind Mount를 이용한 실시간 파일 변경과 Docker Volume을 이용한 데이터 영속성 실습을 통해 컨테이너와 데이터 저장 방식의 차이를 이해할 수 있었다.



또한 Git과 GitHub를 연동하여 프로젝트를 버전 관리하고 원격 저장소에 업로드하는 과정까지 진행하였다.



이번 실습을 통해 개발 환경을 직접 구축하고 관리하는 기본적인 방법을 익힐 수 있었다.



