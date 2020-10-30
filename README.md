# learn-docker
### Docker 이미지 생성 및 배포 방법

##### 우선, Dockerhub에 배포할 시, Dockerhub 로그인

```bash
docker login
```

##### Dockerhub에서 사용하고자 하는 운영체제(ex. Ubuntu)의 이미지를 다운로드

```bash
docker pull ubuntu:18.04
```

##### 다운로드 받은 이미지를 컨테이너로 올리고 접속

```bash
docker run --name [whatever name you want] -it [Image:Tag]
```

##### 작업 후, 새로운 터미널을 열어 현재 활성화된 컨테이너의 ID 확인

```bash
docker ps -a
```

##### 확인한 컨테이너 ID를 커밋하여 새로운 이미지 생성

```bash
docker commit -a "message" [ContainerID] [whatever imagename you want]:[whatever Tag you want]
```

##### 생성한 새로운 이미지에 태그 달기

```bash
docker tag [created image name]:[created image Tag] [DockerhubID]/[created image name]:[created image Tag]
```

##### Tag가 적용된 이미지를 Docker Hub에 배포

```bash
docker push [DockerhubID]/[created image name]:[created image Tag]
```

------

### Github container registry로 배포 방법

##### github profile에서 settings -> Developer settings -> Personal access tokens -> Generate new token 에서 Note(간단한 설명) 작성 및 상단의 repo, write, read, delete 체크박스 모두 체크

![Token_check_list](https://github.com/YounHS/learn-docker/picture/token_check.png)

##### 토큰 값을 txt 파일로 저장 후, ghcr.io 토큰 인증 및 로그인

```bash
cat [토큰 값이 저장된 파일] | docker login ghcr.io -u [GithubID] --password-stdin
```

##### 토큰 인증 및 로그인 후, 하단 명령을 통해 태그 생성 및 배포

```bash
docker tag [created image name]:[created image Tag] ghcr.io/[Github Nickname]/[created image name]:[created image Tag]
```

```bash
docker push ghcr.io/[Github Nickname]/[created image name]:[created image Tag]
```