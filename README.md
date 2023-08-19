# 가이드

test

## ⚙️ 초기 세팅

### 1. fork

organization의 저장소를 `fork`합니다.

### 2. clone

`fork`를 마친 개인 저장소를 `clone`해 로컬 저장소를 생성합니다.

```shell
git clone https://github.com/Chun-gu/sort-by-topic.git
```

### 3. upstream 저장소 추가

organization의 저장소를 `upstream`이라는 이름의 원격 저장소로 추가합니다.

```shell
git remote add upstream https://github.com/cs-group-study/sort-by-topic.git
```

<br/>

## 📌 스터디 당일

### 1. 이전 회차 질답 PR merge

당일 스터디를 진행하기 전, 이전 회차의 질답 `PR`을 `merge`합니다.  
동일한 파일에 각자의 질답을 추가하기 때문에 `conflict`가 발생합니다.  
GitHub 페이지에서 직접 `conflict`를 해결한 뒤 `merge`합니다.  
`merge`된 브랜치는 원격, 로컬 저장소 모두에서 삭제합니다.

### 2. 스터디 진행

스터디를 진행한 뒤, 다음 회차 스터디의 주제를 선정합니다.

<br/>

## 🆕 질답 PR 생성

미리 준비한 질답에 스터디 진행 중 받은 꼬리 질문에 대한 답변까지 더해 다음 스터디 시작 전까지 `PR`을 올려야 합니다.

### 1. 로컬 저장소 최신화

이전에 작업한 브랜치를 삭제합니다.  
`upstream`의 `main` 브랜치를 `pull` 해서 로컬 저장소의 `main` 브랜치를 최신화합니다.

```shell
git checkout main
git branch -D 230823/chun-gu
git pull upstream main
```

### 2. 새로운 브랜치 생성

브랜치 이름은 `일자/이름`으로 합니다. ex) `230823/chun-gu`

```shell
git checkout -b 230823/chun-gu
```

### 3. 질답 작성

주제에 맞는 폴더 내부의 `README.md`에 질답을 작성합니다.

### 4. commit & push

작성한 질답을 `commit` 하고 개인의 원격 저장소(`origin`)에 `push` 합니다.

```shell
git add 230823/README.md
git commit -m "doc: add chun-gu's qna on 230823"
git push origin 230823/chun-gu
```

### 5. PR 생성

`GitHub` 페이지의 개인 저장소의 `230823/chun-gu` 브랜치에서 organization 저장소의 `main` 브랜치로 PR을 보냅니다.
