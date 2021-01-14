# Learn-configuration_mgmt!!!
> Let's learn configuration_mgmt

### IaC

2. ##### 환경 구성 관리

   - **Ansible**

   - Salt

   - Chef

   - Puppet



### Ansible?

> 인프라 관리를 코드 기반으로 자동화하는 툴
>
> IaC를 지향하는 오픈소스 기반의 자동화 관리 툴
>
> 자동 구축/관리 역할을 하는 원격 인프라(서버)에게 명령을 전달하는 방식으로 동작



##### 특징

1. Agentless

   - 원격 서버에 에이전트를 설치하던 기존 IaC 솔루션에서 탈피

   - SSH 기반으로 원격 서버에 명령을 전달하기 때문에 에이전트 필요 X

   - 에이전트 설치 단계를 제거하여 인프라 구축 자동화에 용이

2. 접근 용이성

   - 명령 모음집(Playbook)을 YAML 형식의 파일로 관리

   - YAML의 가독성이 우수하기 때문에 ansible의 진입장벽이 낮음

3. 멱등성

   : 여러번 수행해도 같은 결과를 산출

   - YAML로 관리되는 Playbook을 여러번 수행해도 언제나 같은 결과 산출



##### 용어

1. Master server

   : ansible 명령을 여러 원격 서버에 전달하는 주체

2. 인벤토리(Ansible hosts)

   Master server가 명령을 전달할 원격 서버들의 목록

   /etc/ansible/hosts 파일에 서버들의 목록이 저장되어 있으며, 이를 인벤토리라 명명

3. 플레이북

   원격 서버에 전달할 명령들을 모아둔 명령집

   AKA. 스크립트 파일



##### 설치

> 설치 환경: Ubuntu 18.04 / x86_64

```bash
sudo apt-get update

sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible

sudo apt-get update

sudo apt-get install ansible -y
```

