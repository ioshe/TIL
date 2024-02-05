# docker 기초(1)_run_이미지_도커파일명령어

## 1. 도커 run 명령어 인자 
- ui-for-docker 실행
    ```docker
    docker run -d -p 9000:9000 --privileged -v /var/run/docker.sock:/var/run/docker.sock uifd/ui-for-docker
    ```
    
- **`d`**: 터미널이 컨테이너에 바로 연결되지 않고, 컨테이너가 백그라운드에서 독립적으로 실행되게 한다.
- **`p 9000:9000`**: 호스트의 포트와 컨테이너의 포트를 연결하는 데 사용된다.
- **`-privileged`**: 컨테이너에 추가적인 권한을 부여한다. 기본적으로, Docker는 보안상의 이유로 컨테이너에 제한된 권한을 부여하지만, **`-privileged`** 옵션을 사용하면 컨테이너가 호스트 시스템의 거의 모든 권한을 가지게 된다.
- **`v /var/run/docker.sock:/var/run/docker.sock`**: 볼륨 마운트를 설정한다. 
    - **`v`** 옵션은 호스트 시스템의 파일이나 디렉토리를 컨테이너의 파일 시스템에 마운트하는 데 사용된다. 이 경우, 호스트의 **`/var/run/docker.sock`** 파일(호스트의 Docker 데몬에 대한 소켓 파일)을 컨테이너의 같은 위치에 마운트한다. 이를 통해 컨테이너 내부에서 호스트의 Docker 데몬을 조작할 수 있게 되어, 컨테이너 내부에서 다른 Docker 컨테이너를 관리할 수 있게 된다.

## 2. 도커 이미지 만들기
### 도커 명령어
1. `docker login`
2. nginx에 덧붙여서 만들기
    - `docker pull nginx`
3. 컨테이너 만들기
    - `docker run --name userserver -d -p 80:80 nginx`
4. index.html 파일 수정 후 확인, 새로운 이미지 구성을 위한 변화 적용
    - docker cp <로컬_파일_또는_디렉토리_경로> <컨테이너_이름>:<컨테이너_내부_경로>
    - `docker cp C:\ITStudy\07_docker\bindmount\. userserver:/usr/share/nginx/html` 
5. commit 명령어로 이미지 구성
    - docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
    - `docker commit userserver userimage` 
6. 태그 걸기(버전)
    - docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
    - `docker tag userimage [user_id]/userimage` 
7. push로 hub에 때리기
    - docker push [OPTIONS] NAME[:TAG]
    - `docker push ioshe/minsuimage`


### 도커 이미지를 삭제할 때 발생할 수 있는 문제
1. **컨테이너가 실행 중인 경우**: 도커 이미지가 하나 이상의 실행 중인 컨테이너에 의해 사용되고 있을 때, 해당 이미지를 삭제하려고 하면 오류가 발생한다. 이 경우, 이미지를 삭제하기 전에 해당 컨테이너를 멈추거나 삭제해야 한다.
2. **컨테이너가 이미지에 의존하는 경우**: 이미지를 삭제하려고 할 때, 해당 이미지에 의존하는 중지된 컨테이너가 있으면 삭제가 거부될 수 있다. 이 컨테이너들을 먼저 삭제하거나 **`docker image prune`** 명령어를 사용해 더 이상 사용되지 않는 모든 도커 이미지를 정리해야 한다.


## 3. 도커파일 명령어
### Dockerfile 커맨드 종류
| 커맨드 | 의미 |
| --- | --- |
| FROM | 베이스 이미지 지정 |
| RUN | shell command를 해당 docker image에 실행시킬 때 사용함 |
| CMD | 컨테이너 시작시 실행할 커맨드 설정 |
| LABEL | 이미지에 레이블 지정 |
| EXPOSE | 컨테이너에서 노출하는 포트 번호 설정 |
| ENV | 환경 변수 설정 |
| ADD | 이미지에 파일 복사 |
| COPY | 이미지에 파일 복사 |
| ENTRYPOINT | 컨테이너 시작시 실행할 커맨드 설정 |
| VOLUME | 볼륨이 마운트 될 위치 설정 |
| USER | 커맨드를 실행할 때 사용자 ID 설정 |
| WORKDIR | 커맨드를 실행할 때 작업 디렉터리 설정 |
| ARG | 빌드 시에만 사용되는 변수 설정 |
| ONBUILD | 이 이미지를 베이스로 빌드할 때 커맨드가 실행되도록 하기 |
| STOPSIGNAL | 컨테이너를 중지시킬 때의 시그널 번호 설정 |
| HEALTHCHECK | 헬스 체크를 위한 커맨드 설정 |

### 도커 빌드
- 도커를 빌드하면 Dockerfile에 정의된 내용에 따라 도커 이미지가 생성된다. 
- Dockerfile은 도커 이미지를 만들기 위한 레시피이며, 컨테이너가 실행될 환경, 필요한 의존성 패키지, 실행할 명령 등을 정의한다.