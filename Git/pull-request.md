[참고](https://velog.io/@zansol/Pull-Request-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

아래의 내용은 위의 링크와 동일하다.😀....

# pull request

> 회사 기술블로그 repository를 포크떠서 후기 작성하시고 저한테 PR 보내주세요

## 의미

> 내가 작업한 코드가 있으니 내 브랜치를 당겨 검토 후 병합해주세요 (^0^)/

## Pull Request 하는 이유

- 자연스러운 코드 리뷰를 위해
- push 권한이 없는 오픈 소스 프로젝트에 기여할 때
- pull request는 당장 merge하지 않는 다는 규칙이 있어, 코드에 신경을 쓰게 되고 어떤 작업이 언제 적용 되었는지 파악할 수 있다. 오히려 코드 충돌을 줄일 수 있다.

# 방법

## 1. Fork

- 해당 repository를 자신의 저장소로 Fork 한다.

## 2. Clone, Remote 설정

- fork로 생성한 repository에서 `clone` 혹은 `download` 버튼을 누르고 표시되는 URL을 복사한다.
- `$ git clone <복사한 URL>`
- `$ git remote add post<별명> <복사한 URL>`

  > fork, clone 한 프로젝트는 origin이라는 별명이 기본적으로 추가되어 있어 따로 설정해주지 않아도 된다. 원격저장소로 추가할 때에는 별명을 따로 설정해주어야한다.

## 3. branch 생성

- 내 컴퓨터의 clone 프로젝트 저장소(origin)에서 코드를 수정하거나 추가하는 작업은 branch를 만들어서 진행한다.
- develop branch 생성

  `$ git checkout -b develop`

- branch 선택

  `$ git branch`
  콘솔에서 `main` 과 `develop` 을 확인 할 수 있다.

## 4. 수정 후 add, commit, push

- VS code를 사용하여 코드를 수정한다.
- 작업이 완료되면 github Repository (origin)에 `add`, `commit`, `push` 한다.
  > git add .; git commit -m '커밋제목 작성'; git push origin develop

## 5. Pull Request 생성

- push 완료 후, 자신의 github 저장소에서 `compare & pull request` 버튼이 활성화 되어있는걸 확인할 수 있다.
- 버튼을 선택해 `pull request`를 생성한다.

## 6. Merge Pull Request

- PR을 받은 관리자는 코드 변경내역을 확인하고 `Merge`여부를 결정한다.

## 7. Merge 이후 동기화 및 branch 삭제

- `merge`가 완료되면 로컬 코드와 원본의 코드를 병합하고 최신의 상태를 유지하기 위해 동기화한다.
- `$ git pull post(remote 별명)`
- `$ git brach -d develop`
- 위의 명령어를 통해 동기화하고, 필요없어진 브랜치를 삭제한다.
- 나중에 추가적으로 작업이 또 필요하면, 동기화를 한 뒤, 3~7번을 반복하면서 작업하면 된다.
