# CVS, SVM, Git

## 형상관리란?

**형상 관리(SCM, Software Configuration Management)는 소프트웨어 개발 프로세스 각 단계에서 소프트웨어 변경 사항을 체계적으로 추적하고 관리하는 일련의 활동**

단지 소스코드의 버전 관리만을 하는 것이 아닌 소프트웨어의 생명 주기 동안의 요구 사항, 설계 문서, 소스 코드, UI 문서, Test Case 및 각종 결과물에 대해서 형상을 만들고, 형상들의 관계 및 변경사항, 변경시점 등을
 체계적으로 관리하는 것으로 소프트웨어 개발에서 필수적인 활동 중 하나.
 
## CVS - Concurrent Version System

<img width="417" alt="image" src="https://user-images.githubusercontent.com/36829127/184596089-908197bd-a75c-44d4-8ae3-2c6fda2c0b21.png">

**1990년도에 출시됐으며, OSS(Open Source Software)로 서버와 클라이언트를 구분되어, 개발과정에서 사용하는 파일들의 변경 명세를 관리하기 위한 시스템**

### CVS 특징
1. 오랜 기간 많은 유저들이 사용했으며, 시스템이 안정적
2. **중앙에 위치한 Repository에 파일을 저장하며, 모든 사용자가 파일에 접속할 수 있도록 설계**
(서버는 Unix 기반, Client는 여러 OS에서 접근)
3. CheckOut으로 파일을 복사하고, Commit을 통해 변경사항을 저장
4. 파일의 히스토리를 저장하기 때문에 과거 이력 확인 가능
5. Commit 중 오류가 발생하면 롤백되지 않음
6. 다른 개발자가 작업중인 파일에 덮어쓰기가 방지
7. Repository를 백업하는 것 만으로도 프로젝트의 백업이 될 수 있음
8. 상대적으로 속도가 느림



---


## SVN - Subversion

<img width="405" alt="image" src="https://user-images.githubusercontent.com/36829127/184597436-67859649-0564-4e3a-88a5-49f97a7e79ad.png">

**CVS의 단점을 보완하기 위해 2000년에 만들어졌으며, OSS(Apache)로 서버와 클라이언트를 구분하여, 개발과정에서 사용되는 파일을 관리하기 위한 프로그램**

### SVN 특징
1. 최초 1회에 한해 파일 원본을 저장하고 이후에는 실제 파일이 아닌 **원본과 차이점**을 저장하는 방식
2. 언제든지 원하는 시점으로 복구가 가능함
3. Trunk, Branches, Tags의 폴더로 구성되어 형상을 관리함
4. import, commit, commit log, checkout, revert, switch, update, merge 등의 명령어를 사용함



---


## Git

<img width="470" alt="image" src="https://user-images.githubusercontent.com/36829127/184598910-62d0f3c2-a885-488c-b2ea-488925184a4c.png">


**2005년 리눅스 토르발스가 리눅스 커널의 개발을 위해 만들었으며, OSS(GPL2)로 개발자가 중앙 서버에 접속하지 않은 상태에서도 개발을 할 수 있도록 지원해주는 버전 관리 시스템**

### Git 특징
1. Branching 모델로서, 로컬에 다수의 독립성이 보장되는 branch를 허용하고 쉽게 생성, 병합, 삭제를 지원함
2. 원격 서버 Git Repository에 push하지 않은 채로 여러 branch의 생성이 가능함
3. 로컬 우선 작업을 통해 성능이 SVN, CVS보다 우수함
4. 팀 개발을 위한 분산 환경 코딩에 적합함
5. 파일 암호화 및 체크섬(checksum)을 통한 데이터 보장
6. Staging Area를 통해 서버의 Repository로 업로드함
7. 원격 Repository 장애에도 문제없이 버전 관리가 가능


