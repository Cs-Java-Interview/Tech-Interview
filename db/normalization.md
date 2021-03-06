## 정규화

- 정규화란 : 관계형 데이터베이스에서 **중복을 최소화**하기 위해 데이터를 구조화하는 작업
- **정규화를 하는 이유?**
    - 중복되는 데이터(redundancy)를 줄이고, 데이터의 일관성을 유지함
    - 삽입/갱신/삭제 시 **이상 현상**의 발생 가능성을 줄임
    - 확장성이 있어 새로운 데이터 형식 추가시에도 그 구조를 변경하지 않아도 되거나 일부만 변경
- 단점
    - **연산 시간이 증가 (정규화 과정을 거치면 릴레이션을 분해한다. 이로 인해 테이블을 조회할 때, 여러 테이블 간 조인하여 연산하는 경우가 많아진다)**
- 비정규화
    - 조회 시 조인이 많이 발생하여 이로 인한 성능저하가 나타나는 경우, 비정규화를 진행한다.
- 종류
    - 기본 정규형 : 제1정규형, 제2정규형, 제3정규형, BCNF
    - 고급 정규형 : 제4정규형, 제5정규형

### 이상 현상

: 데이터를 삭제,수정,삽입할 때 논리적으로 오류가 생기는 것

![image](https://user-images.githubusercontent.com/77563814/169877443-35470094-f96f-4164-b1fe-eee293f99516.png)
- **갱신 이상** : 튜플의 속성값을 갱신할 때, 중복된 데이터의 일부만 갱신되는 현상
    - ex) 3번째 튜플인 Kate 학생의 주소를 변경 시, 5번째 튜플도 같은 학생이지만 해당 주소값은 변경이 안됨
- **삽입 이상** : 데이터를 삽입 시, 원하지 않는 값도 함께 삽입하게 되는 현상 (해당 속성은 매번 NULL 입력)
    - ex) 강의를 듣는 학생을 추가시, 강의코드/강의명은 NULL값으로 삽입하게됨
- **삭제 이상** : 한 튜플을 삭제할 때, 다른 데이터까지 연쇄 삭제되는 현상
    - ex) 강의코드 A1인 강의가 삭제될 경우, 해당 강의를 듣는 학생들의 데이터도 함께 삭제됨

### 키

- 슈퍼키(Super key) - **유일성**을 만족하는 키이다. ex) {이름, 학번}, {이름, 학번, 번호}, {아이디, 학번}등
- 후보키(Candidate Key) - **유일성과 최소성**을 만족하는 키이다. ex) {이름, 학번}, {아이디, 학번}
- 기본 키(Primary Key) - **후보키 중에서** 기본적으로 사용하기 위해 **선택한 키**이다. ex) {이름, 학번}
- 대체 키(Surrogate Key) - **기본 키로 선택되지 못한 후보키**이다. ex) {아이디, 학번}
- 외래키(Foreign Key) - 다른 릴레이션의 기본 키를 **참조하는 키**이다.

### 무손실 분해

함수 종속성을 해결하려면 한 개의 릴레이션을 여러 개의 릴레이션으로 분해하는 작업을 수행한다. 이 때 원래 릴레이션의 정보를 잃어 버리지 않게 분해하는 것을 무손실 분해(Non-lose Decomposition)라고 한다.

### 제 1정규형


![image](https://user-images.githubusercontent.com/77563814/169877544-83a6a47c-25bc-4b06-855d-eb734867d169.png)

- 모든 속성을 반드시 하나의 원자값만 가져야 한다

위의 그림의 경우, ‘강의’라는 한가지 속성에 여러 데이터를 가지고 있다. 따라서 한가지 속성에는 한 데이터만 존재하도록 릴레이션을 분해해주어야 한다.

### 제 2정규형

![image](https://user-images.githubusercontent.com/77563814/169877573-764850fa-0e14-4ee9-b1d3-098f901ea5a2.png)


- 제 1정규형에 속하고, 모든 속성이 **기본키에 완전 함수 종속**되면 제2정규형에 속한다.
- 즉, 기본키중에 특정 컬럼에만 종속된 컬럼**(부분적 종속)이 없어야 한다**
- 완전 함수 종속
    - 어떤 속성이 기본키에 대해 완전히 종속일 때
- 부분 함수 종속
    - 어떤 속성이 기본키가 아닌 다른 속성에 종속되거나, 기본키가 여러 속성으로 구성되어 있을경우 기본키를 구성하는 속성 중 일부만 종속될 때

위의 그림의 경우 제 2정규형을 만족하려면, 모든 속성이 기본키에 **완전 함수 종속**되도록 릴레이션을 분해하는 정규화 과정을 거쳐야 한다.

ex) 학생의 강의 점수를 알려면 {학번, 이름}이 아닌, {학번} 혹은 {이름}만으로도 알 수 있다. 이는 기본키를 구성하는 속성 중 일부만 종속인 부분 함수 종속 상태이다. 따라서 해당 릴레이션을 분해해주어야한다.

### 제 3정규형

![image](https://user-images.githubusercontent.com/77563814/169877634-45a780ae-cd40-4b64-9e80-3bb42c892c41.png)


- 제 2정규형에 속하고, 기본키가 아닌 모든 속성이 기본키에 **이행적 함수 종속이 되지 않으면** 제3정규형에 속한다.
- 즉, 기본키가 아닌 속성들 간에 종속 관계가 있으면 안된다.
- 이행적 함수 종속
    - A → B , B → C 인 경우 A → C 가 성립될 때

위의 그림의 경우, 기본키가 아닌 ‘강의’가 ‘강의실’을 결정한다. 이는 이행적 함수 종속이므로 해당 릴레이션을 분해해주어야한다.

### BCNF

![image](https://user-images.githubusercontent.com/77563814/169877675-ba39c2df-8d4e-422e-af41-df6385e9b07f.png)

- 제 3 정규화에 속하고, **모든 결정자가 후보키**이면 BCNF에 속한다.
- 즉, ****기본키가 아닌 속성이 기본키의 속성을 결정지을 수 없다.****

위의 그림의 경우, 강사가 강의를 결정하고 있다.(한 강사가 각자 강의를 담당하고 있다고 가정) 기본키에 속하지 않은 ‘강사’가 기본키에 속한 ‘강의’를 결정짓고 있으므로 해당 릴레이션을 분해해주어야 한다.

### 3NF와 BCNF 차이
- 3NF와 BCNF는 일반 칼럼이 주체(결정자)일 경우 이 릴레이션을 분해해주어야 한다.
- 일반 칼럼이 다른 일반 칼럼에 영향을 준다 : 3NF와 BCNF 모두 위반
- 일반 칼럼이 기본키에 영향을 준다 : BCNF만 위반
