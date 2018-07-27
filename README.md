# GitWorkFlow





### 1.Centralized workflow  
팀 구성원은 중앙저장소를 clone(복제)하여 로컬저장소를 만든 후, 로컬저장소에서 개발하고 파일을 수정하여 커밋(commit)한다.
철수와 영희 2명이 개발을 시작한다. 철수와 영희 둘 중 한명은 중앙저장소를 생성한다. 그리고 철수와 영희 모두 `git clone` 명령어로 중앙저장소를 복제해서 로컬 저장소를 생성한다. 파일을 수정하여 중앙저장소가 아닌 로컬저장소에 커밋하는 것이므로, 중앙저장소의 변경내역을 신경쓸 필요없이 몇번이고 수정과 커밋을 할 수 있다.
그리고 자신의 커밋이력을 중앙저장소에 올려 공유하려 한다면 `git push origin master`명령어를 이용한다. 이 명령어의 뜻은 로컬의 master브랜치를 origin(중앙저장소를 복제할 때 자동 생성된 별칭, 로컬 저장소와 중앙저장소를 연결)의 master브랜치에 동기화 하겠다는 뜻이다. 현재 아무도 중앙저장소에 push한 적이 없으므로 작업은 순조롭다.
문제는 다른 한명이 push를 할 때 발생한다. 나중에 commit을 하는 사람을 영희라고 가정할 때, 영희의 commit 이력은 중앙저장소의 최신 이력을 포함하고 있지 않기 때문에 에러가 발생한다. 이것은 중앙 저장소의 commit 이력을 보호하기 위함이다. 따라서 이 문제를 해결하는 방법은 `git pull`명령으로 중앙 저장소의 변경이력을 자신의 로컬저장소로 내려 받은 후, git add/commit 작업을 다시 해주고 진행하는 방법으로 해결한다. 
여기서 잠시 멈추고 공부를 정리해야 할 것이 있다. `git pull`명령은 중앙저장소의 최신 이력을 내려받는 동작(fetch)과 이를 로컬 이력과 합치는 동작(merge)을 한번에 한다. 또한 push작업시에 --rebase(재배열) 명령어를 주면 불필요하게 git add/commit 작업을 다시 해줄 필요없이 git pull 해온 중앙저장소의 변경이력을 영희의 commit 이력 앞에 끼워 넣는다. 

장점. svn과다르게 로컬에서도 커밋할수있다는점. 즉 네트워크가 필요없다는 것 
단점. 중앙저장소를 최신으로 유지하기위해선 매번 pull해와서 다시 push해줘야함. 


### 2.Feature Branch Workflow

Feature Branch Workflow의 핵심 컨셉은 기능별 브랜치를 만들어서 작업한다는 사실이다. 기능 개발 브랜치는 격리된 작업 환경을 제공하기 때문에 다수의 팀 구성원이 메인 코드 베이스(master)를 중심으로 해서 안전하게 새로운 기능을 개발할 수 있다. 이 워크플로우도 프로젝트의 공식적인 변경 이력을 기록하기 위해서 중앙 저장소와 master 브랜치를 사용한다. 그런데, master 브랜치에 직접 커밋하지는 않고, 새로운 기능을 개발할 때마다 브랜치를 만들어서 작업한다. 보통 animated-menu-items 또는 issue-#1061처럼 의미를 담고 있는 브랜치 이름을 사용한다.
브랜치를 이용하면 격리된 영역에서 안전하게 새 기능을 개발할 수 있을 뿐만아니라, master 브랜치를 손대지 않기 때문에 다른 기능 개발 브랜치를 얼마든지 올려도 된다. 이는 일종의 로컬 저장소 백업 역할을 하기도 한다. 
Feature Branch Workflow의 또다른 장점 중의 하나가 바로 `풀 리퀘스트`이다. `풀 리퀘스트`란 기능 개발을 끝내고 master에 바로 병합하는 것이 아니라, 브랜치를 중앙 저장소에 올리고 master에 병합(merge)해달라고 요청하는 것이다. 이것의 이점은 팀 구성원들과 개발 내용에 대한 의견(코드 리뷰 등)을 나눌 수 있다는 점이다. 예를 들어 주니어 개발자가 업무지시를 받고 어떤 기능에 대한 브랜치를 구현하였다고 하자. 주니어 개발자는 이것을 중앙 저장소에 push하고 팀 구성원들에게 `풀 리퀘스트`를 보낸다. 그러면 팀 구성원들은 해당 코드를 리뷰하고, 수정할 점이나 개선점에 대한 조언을 받을 수 있을 것이다. 또한 이것은 꼭 해당 기능 브랜치가 완성되어야만 보내는 것도 아니다. 따라서 개발 중 막히는 부분이 있을 때, 풀 리퀘스트를 보내서 시니어 개발자에게 개발 방향에 대한 조언도 얻을 수 있을 것이다. 이것은 다른 팀 구성원이 나의 작업내용과 진도를 확인할 수 있는 이점이 있고 팀 구성원간에 변경 내용에 대한 소통을 촉진하여 코드 품질을 높이는 부수 효과도 있다.

장점 기능별 개발로 인해 마스터 브랜치가 안전하다. 풀리퀘스트로 인해 팀구성원간의 소통을 촉진하고, 코드품질을 높일 수 있다.
단점  유연성은 큰 장점이지만, 현장에서 적용할 때 유연성은 독이 될 수도 있다. 특히 팀이 크고, 프로젝트 규모가 크면 브랜치마다 좀 더 특별한 의미를 부여하는 것이 더 낫다. 


### 3. Gitflow Workflow

Feature Branch Workflow보다 복잡하긴하지만, 장점이자 단점이었던 유연성을 대형 프로젝트에도 적용할 수 있는 Workflow이다. Gitflow Workflow도 팀 구성원간의 협업을 위한 창구로 중앙 저장소를 사용한다. 로컬 브랜치에서 작업하고 중앙 저장소에 푸시한다. 단지 브랜치의 구조만 다를 뿐이다.
이 워크플로우의 모든 작업 절차들은 master와 develop 두 개의 브랜치를 대상으로 한다. 새로운 기능은 각각의 브랜치에서 개발하고 백업 및 협업을 위해서 중앙 저장소에 푸시한다. 그런데, master 브랜치에서 기능 개발을 위한 브랜치를 따는 것이 아니라, develop 브랜치에서 딴다. 그리고, 기능 개발이 끝나면 다시 develop 브랜치에 작업 내용을 병합한다. 바꾸어 말하면, 기능 개발을 위한 브랜치는 master 브랜치와는 어떤 상호 작용도 하지 않는다. 그리고 develop 브랜치에 릴리스(release 풀어주다)를 할 수 있는 수준만큼 기능이 모이면(또는 정해진 릴리스 일정이 되면), develop 브랜치를 기준으로 릴리스를 위한 브랜치를 딴다. 이 브랜치를 만드는 순간부터 릴리스 사이클이 시작되고, 버그 수정, 문서 추가 등 릴리스와 직접적으로 관련된 작업들을 제외하고는 이 브랜치에 새로운 기능을 추가 병합하지 않는다. 릴리스 준비가 완료되면 master 브랜치에 병합하고 버전 태그를 부여한다. 이 말의 뜻은 이제 master 브랜치가 애플리케이션 버전 1.0이라는 의미이다. 그리고, 릴리스를 준비하는 동안 develop 브랜치가 변경되었을 수 있으므로 develop 브랜치에도 병합한다. 그 후에 릴리스 브랜치는 삭제한다. 
하지만 세상 일이 어찌 자기 마음대로만 되겠는가. 분명 새로운 운영 환경에 릴리스한 후 문제가 생길 수 있을 것이다. 그러면 문제 해결을 위해 master브랜치를 부모로 하는 임시 브랜치를 만든다. 통상적으로 이름은 `hotfix`브랜치 라고 정의하며 오로지 버그 수정만을 위한 브랜치이고 master를 부모로 하기 때문에 다음 릴리스를 위해 개발하던 작업 내용에는 전혀 영향을 주지 않는다. 패치가 준비되면 master와 develop 브랜치 양쪽에 병합하고, 긴급 해치가 완료 되었으므로 애플리케이션 버전 1.1이라는 새로운 버전 이름으로 태그를 매겨야 한다.

장점 릴리스를 위한 전용 브랜치를 사용하므로 한 팀이 릴리스를 준비하는 동안 다른 팀은 다음 릴리스를 위한 기능 개발을 계속할 수 있으며, 혹시 버그가 발생한 경우에도 긴급 패치를 위한 ‘hotfix’ 브랜치를 이용하므로 다음 릴리스를 위한 기능 개발과 전혀 영향이 없다.
단점 상대적으로 복잡?할 수 있겠다. 

### 4. Forking Workflow

Forking Worflow는 다른 워크플로우와 근본적으로 다르다. 하나의 중앙 저장소를 이용하는 것이 아니라, 개개인마다 서로 다른 원격 저장소를 운영하는 방식이다. 모든 프로젝트 참여자가 개인적인 로컬 저장소와 공개된 원격 저장소, 즉 두 개씩의 Git 저장소를 가지는 방식이다. 모든 코드 기여자가 하나의 중앙 저장소에 푸시하는 것이 아니라, 각자 자신의 원격 저장소에 푸시하고, 프로젝트 관리자만 다른 개발자들의 기여분을 공식 저장소에 병합할 수 있다는 점이 가장 큰 특장점이다. 즉, 프로젝트 관리자는 다른 개발자들에게 공식 저장소에 쓸 수 있는 권한을 주지 않고도 다른 개발자의 커밋을 수용할 수 있다. 프로젝트와 직접 관련이 없는 제 3자뿐만아니라, 아주 큰 규모의 분산된 팀에서도 안전하게 협업하기에 좋은 방법이다. 특히, 오픈 소스 프로젝트에서 많이 사용하는 방식이다. 서버에 만든 공식 저장소로 시작한다는 점은 다른 워크플로우와 같다. 그런데, 다른 개발자가 이 프로젝트에 참여할 때는 이 공식 저장소를 직접 복제하는 것이 아니다.

대신 공식 저장소를 포크(fork)해서 자신만의 원격 저장소를 만든다. 이제 이 복제본 저장소는 개인의 공개 저장소 역할을 하고, 다른 개발자는 이 원격 저장소에 푸시할 수 없다(내려 받는 것은 가능하다). 프로젝트 참여자는 서버측 복제본(포크)을 만든 다음, git clone 명령으로 로컬 저장소를 만든다. 다른 워크플로우처럼 이 로컬 저장소에서 작업을 수행한다.

로컬 저장소의 커밋 이력을 원격 저장소에 푸시할 때는 프로젝트의 공식 저장소가 아니라, 자신의 원격 복제본에 푸시한다. 그 다음 프로젝트 관리자에게 자신의 기여분을 반영해 달라는 풀 리퀘스트를 던진다. 앞서 봤듯이 풀 리퀘스트는 기여한 코드에 대한 의견을 주고 받는 좋은 채널이 된다.

공식 저장소에 기여받은 코드를 병합할 때는, 프로젝트 관리자는 기여자의 변경분을 자신의 로컬 저장소로 내려 받고, 기존 코드 베이스에 부작용을 일으키지 않는 지 확인한 후, 로컬 master 브랜치에 병합하고, 프로젝트 공식 저장소의 master 브랜치에 반영한다. 이제 기여분이 반영된 공식 코드 베이스를 다른 기여자들도 내려 받아 작업을 계속할 수 있다. 각 기여자의 원격 저장소는 다른 기여자와 변경 내용을 공유하기 위한 편의 장치일 뿐이다. 따라서, Feature Branch Workflow나 Gitflow Workflow처럼 새로운 기능 개발을 위해 격리된 브랜치를 만드는 것은 각자의 자유다. 대신 브랜치를 다른 참여자와 공유하는 방법은 다르다. 다른 워크플로우에서는 공식 저장소에 브랜치를 푸시해서 팀 구성원들이 공유했다면, Forking Workflow에서는 나의 브랜치를 다른 참여자들이 자신의 로컬 저장소로 내려 받아 참고하기도 하고 병합하기도 한다.

다른 워크플로우에서는 중앙 저장소와 연결된 단 하나의 origin만 썼다면, Forking Workflow에서는 두 개의 원격 저장소가 필요하다. 하나는 포크한 원격 저장소이고, 다른 하나는 프로젝트 공식 저장소이다. 이름은 아무렇게나 붙여도 되지만, 일반적으로 포크한 원격 저장소는 origin(git clone할 때 자동으로 만들어진다), 프로젝트 공식 저장소는 upsteram으로 붙이는 것이 일반적이다.

$ git remote add upstream https://bitbucket.org/maintainer/repo.git



복제한 로컬 저장소에서 다른 워크플로우처럼 코드를 수정하고, 브랜치를 따고, 변경 내용을 커밋한다.


자신의 원격 저장소에 변경 내용을 올리기 전까지는 변경 내용은 누구에게도 공개되지 않는다. 프로젝트 공식 저장소의 코드 베이스에 새로운 커밋이 있다면 다음과 같이 가져올 수 있다.
개발한 기능을 공개하려면 다음 두 가지 절차를 거쳐야 한다. 첫째, 자신의 원격 저장소에 변경 내역을 올려서 다른 개발자가 볼 수 있도록 한다. origin을 이미 등록해두었으므로 다음 명령만 하면 된다.

$ git push origin feature-branch
메인 코드 베이스가 아니라, 개발자 자신의 서버측 원격 저장소에 올린다는 점이 다른 워크플로우와의 차이점이다.

둘째, 프로젝트 관리자에게 자신의 기여분을 공식 코드 베이스에 반영해 달라고 요청해야 한다. Bitbucket의 ‘풀 리퀘스트’ 버튼을 이용하면, 어떤 브랜치를 제출할 지 정할 수 있다. 보통 이번에 추가한 기능 브랜치를 프로젝트 공식 저장소의 master 브랜치에 병합해 달라고 요청할 것이다.

.  풀 리퀘스트의 코드를 직접 검토한다.
2.  코드를 로컬 저장소로 가져와 수동으로 병합한다.

변경 내용을 로컬 저장소의 master 브랜치에 전부 병합한 후, 다른 개발자들도 접근할 수 있도록 원격 저장소에 푸시한다.

$ git push origin master
프로젝트 관리자의 origin은 프로젝트 공식 저장소의 공식 코드 베이스이므로 기여자가 제출한 신규 기능은 이제 메인 코드 베이스에 포함되었다.

메인 코드 베이스가 변경되었으므로, 프로젝트 참여하는 모든 개발자가 자신의 로컬 저장소를 동기화해서 최신 상태로 만들어야 한다.

$ git pull upstream master

이 글을 통해 어떤 개발자의 기여 활동이 어떻게 프로젝트의 공식 저장소에 반영될 수 있는지를 설명했다. 꼭 공식 저장소가 아니더라도 어떤 저장소에서든 이 방법을 적용할 수 있다. 예를 들어, 서브 팀이 특정 기능을 개발할 때, 메인 저장소를 건드리지 않고 팀 구성원들이 작업 내용을 공유할 수도 있을 것이다.

다른 개발자와 자신의 변경 내용을 쉽게 공유할 수 있고, 어떤 브랜치든 공식 코드 베이스에 병합할 수 있기 때문에, Forking Workflow는 느슨한 팀 구조에서도 강력한 협업 환경을 제공한다.

장점

단점 오픈소스여야한다는것? 
