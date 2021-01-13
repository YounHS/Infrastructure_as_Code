# docker issue solve list
#### docker내의 ubuntu에서 update 불가능

1. 로컬 네트워크의 dns 목록을 확인

   ```bash
   nmcli dev show | grep 'IP4.DNS'
   ```

   

2. docker 관련 파일 생성하거나 수정

   ```bash
   vi /etc/default/docker
   ```

​	상기의 명령어를 통해 docker 파일 생성 후, `DOCKER_OPTS="--dns [0] --dns [1]"` 입력한 후, 저장

​	[0]과 [1] 에는 1단계에서 확인한 DNS 주소를 입력



3. docker service 재시작

   ```bash
   sudo service docke restart
   ```

------

