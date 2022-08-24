## 애자일 방법론이란

<aside>
  
    ❔ 프로젝트를 작은 단위로 개발 후 고객에게 배포하고, 일정 주기마다 피드백을 받아 수정/추가하며 개발해나가는 방식

</aside>

## 애자일 사용 이유?

![image](https://user-images.githubusercontent.com/77563814/186361940-73a7bfb3-02ec-45b8-ade5-2b91be6ecd44.png)

[이미지 출처](https://www.geeksforgeeks.org/software-engineering-classical-waterfall-model/?ref=gcse)

**폭포수 방법론이란**

- 각 단계가 위에서 아래로 물이 떨어지는 것처럼 순차적으로 진행되는 모델이다.
- `요구사항 분석 → 설계 → 구현 → 테스트 → 유지보수` 단계로 절차적으로 진행된다.
- 여러 단계가 병행적으로 진행되거나 거꾸로 진행되지 않는다. 각 단계가 끝나야 다음 단계로 진행할 수 있다.
- 또한, 각 단계를 문서화하여 체계적이고 안정적으로 진행한다.
- 그러나 중간에 요구사항이 변경될 수도 있기 때문에 현실적이지 못한 방법론이다.

![image](https://user-images.githubusercontent.com/77563814/186361972-3f9f943a-034f-4af9-8796-39c83cfaadc1.png)

[이미지 출처](https://www.geeksforgeeks.org/agile-vs-waterfall-project-management/?ref=gcse)

따라서 폭포수 방법론보다 실용적인 애자일 방법론을 사용한다. 애자일은 적은 문서화를 지향하고, 지속적으로 중간에 요구사항을 추가/수정하고 테스트하며 진행한다.

## 애자일 방법 종류

![image](https://user-images.githubusercontent.com/77563814/186362130-a87ff63c-a232-4fc4-a844-d2c9d472fc52.png)

[스크림 이미지 출처](https://www.scrum.org/resources/what-is-scrum)

1. **스크럼 프로세스** 
    - 스프린트 주기로 진행된다. 개발 과정에 대한 모든 주기
    - **스프린트(Sprint)**
        - 작은 기능**에 대해 요구사항 분석/설계/구현/테스트하는 전체 주기**
        - 약 1-2주 동안의 할 수 있는 개발 분량
    - Hotfix (예측되지 않았던 요청)는 스프린트가 아닌 backlog에 쌓아두고, 다음 스프린트에서 진행한다.
2. **칸반**
    - WIP(Work In Progress)기반으로 진행된다. 이슈들은 backlog에 쌓이고, 급한 순서대로 처리한다.
    - 스프린트와 다르게 언제 시작/처리할 지 계획하지 않는다(스프린트처럼 마감일이 없다).
    - Hotfix (예측되지 않았던 요청)는 backlog에 넣어두고, 급한 일이라면 바로 처리할 수도 있다. (마감일이 따로 없고, backlog에서 중요 순서대로 처리하므로)

**Reference**

- [http://frontend.diffthink.kr/2019/07/xp.html](http://frontend.diffthink.kr/2019/07/xp.html)
- [https://shin1303.tistory.com/entry/용어-정리-애자일-방법론이란](https://shin1303.tistory.com/entry/%EC%9A%A9%EC%96%B4-%EC%A0%95%EB%A6%AC-%EC%95%A0%EC%9E%90%EC%9D%BC-%EB%B0%A9%EB%B2%95%EB%A1%A0%EC%9D%B4%EB%9E%80)
