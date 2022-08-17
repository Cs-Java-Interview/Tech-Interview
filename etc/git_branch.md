# Git Branch 5가지

<img width="780" alt="image" src="https://user-images.githubusercontent.com/36829127/185093318-91a22d71-d252-483d-b484-ee82ad9cf54e.png">



## 1. Master Branch


**제품으로 출시될 수 있는 브랜치**

배포(Release) 이력을 관리하기 위해 사용. 즉, 배포 가능한 상태만을 관리한다


<img width="562" alt="image" src="https://user-images.githubusercontent.com/36829127/185093450-c71952cc-9582-4321-842f-775ac0d40ae9.png">


## 2. Develop Branch


**다음 출시 버전을 개발하는 브랜치** 

기능 개발을 위한 브랜치들을 병합하기 위해 사용. 즉, 모든 기능이 추가되고 버그가 수정되어 배포 가능한 안정적인 상태라면 develop 브랜치를 ‘master’ 브랜치에 병합(merge)한다. 

평소에는 이 브랜치를 기반으로 개발을 진행한다.

<img width="518" alt="image" src="https://user-images.githubusercontent.com/36829127/185093665-4b5226eb-021c-46f8-99dd-6d64e620ed02.png">


## 3. Feature Branch


**기능을 개발하는 브랜치**

feature 브랜치는 새로운 기능 개발 및 버그 수정이 필요할 때마다 ‘develop’ 브랜치로부터 분기한다. 

feature 브랜치에서의 작업은 기본적으로 공유할 필요가 없기 때문에, 자신의 로컬 저장소에서 관리한다. 

개발이 완료되면 ‘develop’ 브랜치로 병합(merge)하여 다른 사람들과 공유한다.

1. develop 브랜치에서 새로운 기능에 대한 feature 브랜치를 분기함
2. 새로운 기능에 대한 작업을 수행
3. 작업이 끝나면 develop 브랜치로 병합(merge)
4. 더 이상 필요하지 않은 feature 브랜치는 삭제
5. 새로운 기능에 대한 feature 브랜치를 중앙 원격 저장소에 올림(push)

<img width="595" alt="image" src="https://user-images.githubusercontent.com/36829127/185094000-76bec2b5-c7c4-4f5f-a5f8-7d00197b720a.png">


## 4. Release Branch

**이번 출시 버전을 준비하는 브랜치**

배포를 위한 전용 브랜치를 사용함으로써 한 팀이 해당 배포를 준비하는 동안 다른 팀은 다음 배포를 위한 기능 개발을 계속할 수 있다. 

즉, 딱딱 끊어지는 개발 단계를 정의하기에 아주 좋다. 

예를 들어, ‘이번 주에 버전 1.3 배포를 목표로 한다!’라고 팀 구성원들과 쉽게 소통하고 합의할 수 있다는 말이다.


<img width="588" alt="image" src="https://user-images.githubusercontent.com/36829127/185094446-7ebd895e-c0bc-41ad-b0a4-089c6bf5a05e.png">



## 5. HotFix Branch

**출시 버전에서 발생한 버그를 수정하는 브랜치**

배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우, ‘master’ 브랜치에서 분기하는 브랜치이다.

‘develop’ 브랜치에서 문제가 되는 부분을 수정하여 배포 가능한 버전을 만들기에는 시간도 많이 소요되고 안정성을 보장하기도 어려우므로 바로 배포가 가능한 ‘master’ 브랜치에서 직접 브랜치를 만들어 필요한 부분만을 수정한 후 다시 ‘master’브랜치에 병합하여 이를 배포해야 하는 것이다. 


<img width="514" alt="image" src="https://user-images.githubusercontent.com/36829127/185094771-0d60f59b-c96d-4de5-b1c8-5da8708f2af8.png">



----

참고 : 

https://velog.io/@hi980506/Github-Git-브랜치의-종류-및-사용법

https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html
