# docker 기초(2)_마운트_yaml분석

## 1. 도커 마운트
### **1. 바인드 마운트 (Bind Mount):**

- 호스트 파일 시스템과 컨테이너 내 경로를 직접 연결, 실시간으로 호스트 변경사항 반영한다.
- 개발 및 테스트 환경에서 파일 및 디렉토리 공유에 주로 사용한다.

### **2. 볼륨 마운트 (Volume Mount):**

- 도커 엔진 관리 볼륨을 사용해 데이터 공유, 호스트 시스템과 독립적으로 컨테이너 내 마운트한다.
- 데이터 영구 저장 및 관리에 사용, 데이터베이스 파일이나 로그 파일 보관에 유용하다.

### 3. 명령어
```bash
#!/bin/bash
docker run --name <생성하는컨테이너_이름>  -d  -p  <포트포워딩>  -v  <호스트경로>:<컨테이너경로>  <이미지명>
```

## 2. yaml 파일 분석

### 버전 (Version)
```yaml
version: "1"
```
- Docker Compose 파일의 버전을 지정한다.

### 서비스 (Services)
```yaml
services:
  mysql000ex11:
    image: mysql:8
```
- Docker Compose 파일에서 services 섹션은 실행하려는 컨테이너를 정의한다.
- **mysql000ex11**: MySQL 서비스의 이름이다.
- **image**: 사용할 Docker 이미지를 지정한다. 여기서는 MySQL의 8번 버전을 사용한다.

### 네트워크 (Networks)
```yaml
networks:
    - wordpress000net1
```
- **networks**: 이 서비스가 연결될 네트워크를 지정한다.
- **`wordpress000net1`** 네트워크를 사용하여 WordPress 서비스와 통신한다.

### 볼륨 (Volumes)
```yaml
volumes:
    - mysql000vol11:/var/lib/mysql
```
- **volumes**: 데이터 지속성을 위해 사용됩니다. 
- **`mysql000vol11`** 볼륨을 MySQL 데이터 디렉토리`(/var/lib/mysql)`에 마운트합니다.

### 환경 변수 (Environment)
```yaml
environment:
    MYSQL_ROOT_PASSWORD: myrootpass
```
- **environment**: 서비스에 필요한 환경 변수를 정의한다.
- 여기서는 MySQL의 루트 패스워드, 데이터베이스 이름, 사용자 이름 및 패스워드를 설정

### WordPress 서비스
```yaml
wordpress000ex12:
    image: wordpress
```
- **wordpress000ex12**: WordPress 서비스의 이름이다.
- **image**: 여기서는 최신 WordPress 이미지를 사용한다.

### 포트 (Ports)
```yaml
ports:
    - 85:80
```
- **ports**: 호스트와 컨테이너 간의 포트 매핑을 정의한다. 
- 호스트의 85번 포트를 컨테이너의 80번 포트에 연결한다.



```yaml
User
version: "1"
services:
  mysql000ex11:
    image: mysql:8
    networks:
      - wordpress000net1
    volumes:
      - mysql000vol11:/var/lib/mysql
    restart: always
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
    environment:
      MYSQL_ROOT_PASSWORD: myrootpass
      MYSQL_DATABASE: wordpress000db
      MYSQL_USER: wordpress000kun
      MYSQL_PASSWORD: wkunpass

  wordpress000ex12:
    depends_on:
      - mysql000ex11
    image: wordpress
    networks:
      - wordpress000net1
    volumes:
      - wordpress000vol12:/var/www/html
    ports:
      - 85:80
    restart: always
    environment:
      WORDPRESS_DB_HOST: mysql000ex11
      WORDPRESS_DB_NAME: wordpress000db
      WORDPRESS_DB_USER: wordpress000kun
      WORDPRESS_DB_PASSWORD: wkunpass
networks:
  wordpress000net1:
volumes:
  mysql000vol11:
  wordpress000vol12:
````