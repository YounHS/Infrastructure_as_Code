# How to set playbook???
> Let's learn, how to set playbook

### Ansible-playbook?

> 모듈 호출의 중심에 있는 ansible code
>
> YAML로 작성



##### Playbook tutorial

1. YAML example (test.yml)

   ```yaml
   --- <1>
   - name: yaml test <2>
     hosts: test-12 <3>
     ...
   ```

   - <1>: YAML 문서임을 알림

   - <2>: 로그에 표시되는 이름

   - <3>: 배포 대상 호스트 지정. all로 설정 시, 인벤토리로 정의된 모든 호스트를 대상으로 함

     

2. Inventory example (hosts.ini)

   ```yaml
   test-12 ansible_host=test@192.168.0.15 <1>
   
   [slave-group1] <2>
   test-12
   ```

   - <1>: [서버이름] ansible_host=[서버유저명]@[서버IP]

   - <2>: 여러 개의 호스트르르 그룹화

     

3. ansible-playbook test

   ```shell
$ ansible-playbook -i hosts.ini test.yml
   [WARNING]: Invalid characters were found in group names but not replaced, use
   -vvvv to see details
   
   PLAY [first test] **************************************************************
   
   TASK [Gathering Facts] *********************************************************
   ok: [test-12]
   
   PLAY RECAP *********************************************************************
   test-12                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```
   
   - ansible-playbook -i [Inventory file] [playbook yml]
   - YAML로 관리되는 Playbook을 여러번 수행해도 언제나 같은 결과 산출



##### Playbook module (hosts.ini)

1. To do...




[참고자료](https://wickso.me/ansible/basic/)

------

##### To do

> playbook module 컨텐트 정리
