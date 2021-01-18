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

   - <2>: 여러 개의 호스트를 그룹화

     

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



##### Playbook module

1. example

   ```shell
   $ ansible test-12 -m ping -u test
   ```

   - 상기의 명령어는 ping 테스트를 수행

   - ansible [호스트 그룹명] -m ping -u [접근할 때 사용할 호스트 계정명]

   - result

     ```shell
     192.168.0.12 | SUCCESS => {
     	"ansible_facts": {
     		"discovered_interpreter_python": "/usr/bin/python"
     	},
     	"changed": false,
     	"ping": "pong"
     }
     ```

     

2. 자주 사용하는 모듈

   - file: 파일과 디렉터리 상태 작업
   - copy: 파일을 작업 대상에게 전송
   - lineinfile: 기존 파일을 행 단위로 수정
   - command: 임의의 명령 실행
   - raw: python을 거치지 않는 명령을 저레벨로 실행
     - SSH를 통해 명령어 실행
     - 가능한 사용을 지양하는 것이 좋음
       - ansible에 대응하는 python을 설치하기 위해 사용
       - 네트워크 기기 등, python이 내장되지 않은 환경 조작을 위해 사용



##### ansible variable

- 환경에 따라 상이한 값을 사용하기 위함

  - 사람이 정한 변수와 호스트가 상태에 따라 자동으로 설정하는 변수로 구분

  - 코드 변경 없이 다양한 환경에서 playbook 사용 가능

    **-> playbook 범용성 향상**

- 반복되는 값의 정의를 정리하기 위함

  - 파일 경로가 반복되는 경우에 유용

  - 입력 시, 실수를 방지하고 값을 수정할 때도 한 곳만 변경

    **-> playbook의 유지보수성 향상**

1. role

   - 알파벳 대소문자, 숫자, _(언더바) 사용가능
   - 숫자 시작 불가
   - 변수 이름에 -(하이픈), 공백, 한글 사용 불가
   - 파이썬 예약어 사용 불가

   

2. definition

   변수를 정의하는 4개의 레이어

   - 모듈이 자동으로 정의(Facts)
   - 인벤토리(그룹/호스트) 레벨에서 정의
   - ansible이 실행될 때 정의
   - playbook에서 정의

   

3. option (in shell)

   - `--extra-vars ( -e )` 옵션 사용

   - 띄어쓰기로 구별하는 key=value 형식

     ```shell
     $ ansible-playbook -i hosts -e 'nginx_version=1.10.2 nginx_user=nginx' test.yml
     $ ansible-playbook -i hosts -e '{"nginx_post": 8080}' test.yml
     $ ansible-playbook -i hosts =e '@extra-vars.yml' test.yml
     ```

   - line 1: `--extra-vars ( -e )` 옵션 사용 및 띄어쓰기로 구별하는 key=value 형식

   - line 2: 'line 1' 방식으로 사용하면 모두 string으로 다뤄야하므로, 숫자나 boolean 값 등을 다루기 위해 JSON 형식으로 정의

   - line 3: @를 붙이면 YAML 형식으로 변수를 정의한 파일 읽기 가능

     

4. option (in playbook)

   - vars 지시자로 직접 정의

     ```yaml
     vars:
     	nginx_port: 8080
     ```

   - 외부 파일에서 변수 읽어들이기

     ```yaml
     vars_files:
     	- test_vars.yml
     ```

     

5. priority

   우선순위 높음 -> 낮음

   - 엑스트라 변수 (명령 실행 시, `-e` 로 전달한 것)
   - Task 변수 (지정한 Task에서만 유효)
   - 블록 변수 (Task를 모아놓은 블록에서만 유효)
   - role의 vars에서 변수를 정의한 파일 / include_vars 모듈로 읽은 변수를 정의한 파일
   - playbook의 vars_files 지시자로 읽은 변수를 정의한 파일
   - playbook의 vars_prompt 지시자로 입력한 변수
     - playbook 실행 시, prompt에서 변수의 설정값을 수동으로 입력하는 vars_prompt 지시자도 있으나, prompt를 주로 사용하게 되면 playbook을 자동으로 실행할 수 없게되므로 이 지시자는 제한적 사용 필요
   - playbook의 vars로 정의한 변수
   - set_fact로 정의한 값
   - register로 정의한 값
   - Host의 Fact 정보 (setup이 수집한 정보)
   - playbook directory에서 host_vars의 호스트 변수를 정의한 파일
   - playbook directory에서 group_vars의 그룹 변수를 정의한 파일
   - inventory directory에서 host_vars의 호스트 변수를 정의한 파일
   - inventory directory에서 group_vars의 그룹 변수를 정의한 파일
   - inventory 파일 (Dynamic 포함)에서 정의한 변수
   - role의 default에서 default 변수로 정의한 파일



[참고자료](https://wickso.me/ansible/basic/)

------

##### To do
> Jinja2 정리
