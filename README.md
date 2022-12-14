# to be server dev

- 서버 개발자가 하는 일
    1. TR(transaction) or API 만들기, 
    2. 배치 만들기 (서버 내부적으로 실행하는 것)
    3. DB table 만들기

- 개발 환경 만들기
    - 서버, DB 만들기, 데이터 넣기, 테스트 환경 만들기 (사실 주니어 개발자의 영역은 아님, 하지만 스타트업을 간다면 다를 수도)
    - 
    
1. 서버 만들기 (AWS 이용)
    - 인스턴스 시작
        - AMI (Amazon Machine Image) 선택
            - Ubuntu Server 18.04 LTS
            - Ubuntu 선택 이유: Linux 보다 검색해서 나오는 레퍼런스가 더 많음.
        - 인스턴스 유형 (서버 유형)
            - t2.micro (1 cpu, 1 GiB 메모리)
        - 보안 그룹 구성
            - SSH, HTTP, HTTPS 등등에 대해 소스(사용자 범위)를 설정함으로써 어떤 IP의 접근을 수락/거부를 할지 거를 수 있음
            - SSH / 위치 무관 으로 설정.
                - SSH는 자체적으로 key가 있어야 접근이 가능함. ‘위치 무관’으로 설정해도 괜찮음
                - SSH의 고유 포트번호는 22이다.
        - 예상 요금 알림 받기
            - 프리티어의 한도를 어느 정도 넘으려고 할 때 돈을 지불할 수 있다는 메일이 온다.
            
2. 서버 접속하기
    - putty 설치
    - Saved Session : 퍼블릭 IPv4 주소, port, connection type을 입력하여 ‘aws_server’ 라는 이름으로 세션 저장
    - private key 등록(?)
        - Connection → SSH → Auth 에서 Authentication parameters 항목에 ppk를 등록
        - 기존 pem key를 등록하지 못함. 변환해야함.
            - puttyGen을 통해 변환한다. Conversion → Import Key → Save private key (ppk 저장)
    - Open
        - root 계정 밖에 없음. login id는 ubuntu 이다.
        - root로 전환하고 싶으면
        
        ```bash
        sudo su
        ```
        
    
3. 프로젝트 생성
    - Pycharm을 통해 Django 프레임워크를 활용한 프로젝트를 새로 생성
    - 기본적으로 실행시 로컬 주소에서 성공적으로 실행됨을 볼 수 있음
    
4. 프로젝트를 github에 올리기
    - 서버에 올린다 라고 하지만 엄연히 좀 다름. 깃허브에 올린 프로젝트를 서버에서 가져다 사용하는 것. 로컬 → 깃헙 → 서버 이런 느낌?
    - 레포지토리 생성
    - 테스트
        
        ```bash
        echo "# server_dev" >> README.md
        git init
        git add README.md
        git commit -m "first commit"
        git branch -M main
        git remote add origin https://github.com/kimgun95/server_dev.git
        git push -u origin main
        ```
        
5. Sourcetree 로 형상관리
    - 레포지토리 add
    - .gitignore 설정하기
        - 설정 → 고급 → 저장소별 무시 목록 편집
        - .gitignore
            
            /venv/*
            /.idea/*
            /**pycache**/
            
    - 프로젝트 파일 add/commit/push

6. Server에서 git 프로젝트 가져오기
    1. 폴더 생성 후 git init 등등 을 통해 프로젝트 pull
    2. 필요 라이브러리 설치
        
        ```bash
        sudo apt update
        sudo apt install python3.9
        sudo apt install python3-pip
        pip3 install virtualenv
        sudo apt install virtualenv
        ```
        
        pip는 파이썬에서 util, package 설치에 필요한 것
        
    3. 가상 환경 만들기/액티브 시키기/비활성화
        
        ```bash
        virtualenv venv --python=python3.9
        source venv/bin/activate
        deactivate
        ```
        
    4. 패키지 설치하기
        - 서버에는 Django와 같은 패키지가 설치되어 있지 않음
        - 로컬 환경에 이미 설치된 패키지들을 설치해야 함
        - 로컬에서 설치된 패키지 목록 확인, 목록을 txt 파일로 저장
        
        ```bash
        pip freeze
        pip freeze > requirements.txt
        ```
        
        - 서버에서 git에 있는 requirements.txt를 pull한 뒤 패키지 설치
        
        ```bash
        pip install -r requirements.txt
        ```
        
        → 파이썬 버전이 3.6이 기본으로 설치되는 바람에 패키지 설치 진행이 되지 않음.
        
        고군분투 끝에 로컬 프로젝트에서 사용하기로한 python3.9 버전으로 설치 및 변경 완료 후 설치를 끝냄.
        
        지금 생각해보면 venv 환경만 3.9로 변경하면 됐는데 전체 환경을 모두 3.9로 변경함.
        
    5. 서버 실행시키기
        
        ```bash
        python manage.py migrate
        python manage.py runserver 0.0.0.0:8000
        ```
        
        하지만 접속이 되지 않을 것.
        
        AWS 서버에서 보안에 인바운드 규칙에 SSH만 추가해 놓았기 때문.
        
        → putty를 통한 접속이죠 이건!
        
        사용자 지정 TCP, 포트번호 8000 을 추가한다.
        
        → 브라우저에서 13.125.18.51:8000 페이지 접속을 의미하죠!
        
        그러나 이번엔 Django에서 접속을 막을 것이다. 아래와 같은 화면 볼 수 있음.
        
        ![django_disallowed](https://user-images.githubusercontent.com/54833128/190974490-5b1838cc-f72f-407e-98b2-77bf8dc5ff56.png)
        
        Django에서 settings.py 파일에서 아래처럼 호스트 추가. 모든 호스트를 허용.
        
        ```python
        ALLOWED_HOSTS = ["*"]
        ```
        
        서버 실행 성공
        
        ![server_acess_success](https://user-images.githubusercontent.com/54833128/190974484-2d9caac1-a5df-4188-935d-c0005038bf25.png)