\# Mission 1 - Docker 개발 환경 구축 및 실습



\## 1. 실습 목표

\- 개발 도구 설치 및 버전 확인

\- PowerShell과 Ubuntu 파일 및 폴더 관리

\- Docker 이미지와 컨테이너 실행 및 관리

\- Docker로 웹페이지 실행

\- Bind Mount와 Volume을 이용한 데이터 공유 및 영속성 확인



\## 2. 개발 환경



| 도구 | 버전 |

|---|---|

| Docker | 29.9.2 |

| Git | 2.56.0.windows.4 |

| VS Code | 1.144.0 |

| PowerShell | 7.6.9 |

| WSL | 2 |

| Ubuntu | 24.04.5 LTS |



\## 3. 실습 결과

\- Docker Hello World 실행 성공

\- Ubuntu 컨테이너 실행 및 파일 확인 성공

\- Nginx 웹페이지 빌드 및 접속 성공

\- Bind Mount 파일 변경 반영 확인

\- Volume 데이터 영속성 확인 성공



\## 4. Docker 기본 명령어

docker run hello-world

docker ps

docker ps -a

docker images



docker logs mission1-ubuntu

docker inspect mission1-ubuntu

docker stats --no-stream mission1-ubuntu

docker start mission1-ubuntu

docker exec mission1-ubuntu cat /mission1-test/container-test.txt

docker stop mission1-ubuntu



\## 5. 웹페이지 Docker 실습

이미지 빌드 명령어:

docker build -t mission1-web .

컨테이너 실행 명령어:

docker run -d --name mission1-web-container -p 18881:80 mission1-web

브라우저 주소:

http://127.0.0.1:18881/



결과: Hello, Docker! 웹페이지 표시 성공.



\## 6. Bind Mount 실습

호스트 파일을 컨테이너 내부에 연결하는 명령어:

$htmlPath = (Resolve-Path .\\index.html).Path

docker run -d --name mission1-bind-container -p 18882:80 --mount "type=bind,source=$htmlPath,target=/usr/share/nginx/html/index.html,readonly" nginx:alpine

브라우저 주소:

http://127.0.0.1:18882/



결과: 호스트에서 수정한 HTML 파일이 컨테이너에도 반영되는 것을 확인.



\## 7. Docker Volume 실습

docker volume create mission1-data

docker run --name mission1-volume-first -v mission1-data:/data ubuntu:24.04 bash -c "echo 'Volume persistence test!' > /data/volume-test.txt \&\& cat /data/volume-test.txt"

docker rm mission1-volume-first

docker run --name mission1-volume-second -v mission1-data:/data ubuntu:24.04 cat /data/volume-test.txt

실행 결과:

Volume persistence test!



첫 번째 컨테이너를 삭제한 뒤 새 컨테이너에서도 기존 데이터가 유지되는 것을 확인.



\## 8. Bind Mount와 Volume의 차이

Bind Mount: 내 컴퓨터의 특정 파일이나 폴더를 컨테이너와 연결한다.

Volume: Docker가 관리하는 저장 공간으로, 컨테이너를 삭제해도 데이터가 유지된다.



\## 9. 문제 해결 기록



문제 1: Windows 경로를 Bind Mount에 연결할 때 오류가 발생했다.

해결: Resolve-Path로 절대 경로를 구하고 Docker의 --mount 옵션을 사용했다.



문제 2: 브라우저 접속 로그에 favicon.ico 404 오류가 표시되었다.

해결: 파비콘 파일이 없어서 발생한 오류이며, 웹페이지 표시에는 영향이 없었다.



\## 10. 스크린샷



웹페이지 실행 화면을 캡처하여 screenshots/mission1-web.png 파일로 저장한다.



