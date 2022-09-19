# 서버 개발 중 찾아본 개념

1. sudo
    - super user do → substitute user do
        
        unix의 기능이 확장되며 ‘다른 사용자의 권한으로 실행’으로 불리기도 한다.
        
    - 정의
        
        각종 명령어 맨 앞에 sudo를 붙이면 그 명령어는 root 권한, 즉 최고 관리자 권한으로 실행됨
        
        관리자 계정의 비밀번호로 직접 로그인하는 su와 달리 sudo는 사용하는 당사자의 비밀번호를 사용.
        
    - 상세
        - sudoers
            
            sudo 명령어는 /etc/sudoers 설정 파일에 명시되어 있는 user만 사용 가능
            
            - 수정을 하고 싶을 때
                1. 계정
                    
                    root 계정으로 전환
                    
                2. 수정
                    
                    visudo 명령어 사용 (정석적인 방법)
                    
                    ```bash
                    visudo -f /etc/sudoers
                    ```
                    
                    혹은 chmod를 통한 파일 권한 변경
                    
                    하지만 이 방법은 추천하지 않는다고 한다.
                     
            - 그 외 sudoers내 변경 방법은 [링크](https://whitewing4139.tistory.com/74) 참조.
---
2. apt
    - Ubuntu, Debian 및 파생 제품에서 deb 패키지를 설치, 업데이트, 제거하기 위한 명령줄 유틸리티
    - apt-get, apt-cache에서 자주 사용되는 명령으로 구성
    - 최종 사용자에게 적합, apt-get 명령의 일부 기능 없음, apt-get의 결함 중 일부를 수정하여 설계됨
    
    그럼 apt-get은 뭔가?
    
    - Debian 기반 시스템에서 널리 사용되는 CLI 패키지 관리 도구
    - 패키지를 설치, 업데이트, 제거 할 수 있음
    
    apt-cache
    
    - 새 패키지 검색
    
    dpkg
    
    - 시스템에 설치된 패키지 조회
    
    결국 큰 차이점은 뭐냐?
    
    - 보기 좋은 진행률 표시 제공
    - 업그레이드 필요한 패키지 목록 조회 가능
    - apt-get, apt-cache, dpkg -l 의 기능 결합
    - 새로운 명령어 제공 ex) apt list, apt edit-sources
---
3. 가상환경
    - 파이썬 가상환경이란?
        
        현재 설치된 파이썬 환경과 다른 독립된 환경
        
        파이썬을 사용하다 보면 여러가지 패키지를 설치해야 함. 이때 패키지 버전이 다르면 에러가 발생. 가상환경을 사용하지 않으면 다수의 프로그램이 하나의 파이썬 개발환경을 이용해야 함.
---
4. WSGI
    
    Web Server Gateway Interface
    
    웹 서버와 웹 프레임워크 사이에서 통신을 담당하는 인터페이스, 즉 웹 서버와 Django 사이 통신을 담당
    
    ![architecture](https://user-images.githubusercontent.com/54833128/190975232-f0e965ef-b8cc-4748-a912-7354f5e0447d.png)
    [https://uwsgi-docs.readthedocs.io/en/latest/](https://uwsgi-docs.readthedocs.io/en/latest/)
---
