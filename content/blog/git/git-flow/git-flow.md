---
title: git의 효율적인 관리 전략 Git-flow.
date: "20201004"
description: "현재 근무하는 회사에서 Jira와 함께 Git-flow를 사용해서 Git-flow를 알게 되었는데, 개념을 정리해보며 함께 이해도를 높혀보자!"
---

# Git-flow
 &ensp;필자는 현재 근무하는 회사에서 Jira와 함께 Git-flow를 사용해서 Git-flow를 알게 되었다. 처음엔 생소했지만 간단하게 정리된 설명을 읽어보니 쉽게 이해가 가능했고, 바로 프로젝트에서 사용하고 있는 flow를 따라갈 수 있었다. 이 내용을 한번 더 정리하면서 이해도를 높히고, git-flow가 어렵다고 생각하는 사람들이 글을 보며 쉽게 이해 할 수 있도록 정리해보려고 한다. 
 ---

## 1. Git-flow? 그게 뭔데? 왜 필요한거야?
 &ensp; 이번 장의 제목은 내가 개발을 공부하면서 어떠한 기술이나, 주제에 대해 처음 들었을 때 가장 먼저 하는 질문이다. 그 기술이 무엇인지? 왜 필요한지, 그  기술이 하는 일과 도입 이유를 이해하면 그 이후의 세세한 부분들을 받아들일때, 조금 더 수월하게 수긍 할 수 있던 것 같다. 그래서 Git-flow의 세부적인 내용을 알아가기 전에, 어떤 역할을 하는 것 인지, 왜 필요한지 알아보고 가자. 
 
 &ensp; Git-flow는  git을 사용할 때 효율적으로 branch를 나누고 버전을 관리하기 위해 Vincent Driessen가 고안한 하나의 branch 전략이다. branch 전략이 필요한 이유는 무엇일까? git을 보통 혼자 사용할 때는 기능을 추가하거나, 큰 변화가 있을 때 자신이 편한 네이밍을 쓰고 원하는 대로 branch를 만든후 개발해 마스터에 merge한다. 이러한 과정을 여러 사람들이 아무런 규칙 없이 하게 된다면 어떻게 될까? git의 log는 읽기 힘들고, 어떠한 branch를 왜 추가했는지 알기 어려울 것이다. 또한 실제 서비스를 하고 있는 경우 master에 직접 merge를 하게 된다면 실수가 있을 경우 서비스에 차질이 생길 수 있다. 이러한 문제를 해결하고, 기능 개발부터 배포까지, 효율적인 branch관리를 위해 제시된 전략중 하나가 Git-flow이다. 
 ---

## 2. Git-flow의 5가지 큰 branch
 &ensp; Git-flow 전략은 feature-branches, develop, release, hotfixes, master로 각 branch의 종류를 5가지로 나누어 관리한다. 각각의 branch는 개발, 배포 까지의 과정에서 서로 다른 역할을 담당한다. 개발 단계부터 부터 각각의 branch가 merge되어가는 과정은 feature branches -> develop -> release -> master 순으로 이루어진다. 추가로 hotfix branch가 있는데 이 브렌치는 아래 그래프에서 볼 수 있듯이 master에서 branch를 따서 개발 후 develop과 master에 merge를 한다.  
 
  ![git](../assets/git-flow.png)

이제 큰 흐름을 알았으니 각각의 branch에서 어떤 작업을 하고, 다음 branch로 merge하는지 알아보자. 각각의 branch에서 하는 일은 다음과 같다.

| branch | 역할 | 비고 |
|--------|------|-------|
| feature-branches | 새로운 기능을 추가하기 위해 생성하는 브랜치들로써 여러 브랜치가 존재할 수 있다. | 이 브랜치는 develop으로 부터 만들어지고 개발을 마치고 다시 develop으로 merge된다. 이름은 feature/[피쳐이름] 꼴로 지어진다. feature 브랜치는 develop에 머지된 후 삭제 한다. |
|develop|feature가 생성되어 나가는 branch이며, 기능이 개발 된 뒤에 merge되는 곳이다. 즉 다음에 출시될 버전을 개발하고, 합치는 브랜치이다.| 이 브랜치는 처음에 mater로 부터 만들어졌으며 하나만 존재한다. 그리고 이름은 그대로 develop이다. |
|release|개발이 끝난 후 출시될 버전이 머지되는 브랜치이다. 이 브랜치에서 주로 QA가 이루어 지고, QA담당자가 각 feater의 기능을 테스트한다.  | 이 브랜치는 develop으로 부터 생성되고, 출시 버전마다 release-0.0.0의 꼴로 이름 뒤에 버전이 붙는다.  release 브랜치는 master에 머지한 후 삭제 한다. |
|master| 최종적으로 제품으로 출시되는 브랜치로써 release에서 각종 테스트가 끝난 후에 master로 merge된다. |사용자가 사용하게 되는 버전이다.|
|hotfix|master에서 급하게 고쳐야 하는 버그가 생겼을 때 이를 수정하기 위해서 master에서 생성해내는 branch이다. |이 브랜치는 master에서 생성되어 master와 develop에 merge된 후 삭제 된다.|

 
---
## 3. git-flow의 단점.
 &ensp; git-flow는 정말 유용한 전략이지만 항상 Git-flow가 모든 부분에서 좋을까? 그렇지는 않다, git-flow는 여러 git 전략 중 하나이고 단점도 존재한다.
 1. 기본적으로 여러 branch들을 생성해서 관리해야 하기 때문에 여러 불필요한 브랜치를 생성해야 한다는 문제가 있다.
    * 별 다른 작업을 취하지 않는 master 등의 branch 를 관리해야 한다.
 2. 몇몇 branch에서 애매한 branch를 생성해야 하는 경우도 있다.
    * 작업을 하면서 이슈가 들어올 때 hotfix가 아닌 미미한 버그를 수정할 때는 develop에서 작업을 하게 되는데, 이럴 경우 feature로 네이밍을 하기 애매한 상황이 존재한다.

---
## 4. 프로젝트에서의 작업 과정.

git flow를 이용해 프로젝트를 작업하는 방식은 다음과 같다. 

1. 개발할 feature를 정의해 이슈로 만든다. 
2. develop에서 feature branch를 feature/[피쳐이름] 꼴로 생성한다. 
3. 열심히 개발을 진행한다 ( 뚝딱뚝딱 )
4. 개발이 완료된 feature branch를 develop에 pull request를 생성해 상호간에 코드리뷰를 한후 merge하고 feature를 삭제한다.
5. 많은 feature가 merge된 develop에서 release-x.x.x를 생성한다. 
6. release-x.x.x에서 QA와 여러 테스트를 진행한다.
7. 최종적으로 문제가 없는 release를 master로 merge한 후 release-x.x.x를 삭제한다.
8. 배포!

## 마치며.

&ensp; git branch전략인 git-flow를 알아보았다. git-flow어려운 개념이 아니라 기능 개발버전 부터 최종버전까지의 개발 과정에 맞추어 브랜치를 나누어 관리하는 방법이다. 
이 글을 쓰며 이전에 학교 수업에서 조별과제를 할 때, branch관리를 하지 않고 단순히 원하는대로 branch를 따고, merge를 하고, 심지어 master에서 바로 함께 같은 부분을 작업해서 충돌을 해결하느라 많은 시간을 썼던 것이 기억났다. 그때 이러한 전략을 알았더라면 더 쉽게 관리할 수 있었을 것 같다는 생각이 든다.   

 