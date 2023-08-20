# 가이드

## ⚙️ 초기 세팅

organization의 저장소를 `clone`해 로컬 저장소를 생성합니다.

```shell
git clone https://github.com/cs-group-study/interview-study-archive.git
```

<br/>

## 📌 스터디 진행 순서

### 1. 이전 회차 질답 PR merge

- 당일 스터디를 진행하기 전, 이전 회차의 질답 `PR`을 `merge`합니다.
- 동일한 파일에 각자의 질답을 추가하기 때문에 `conflict`가 발생합니다.
- GitHub 페이지에서 직접 `conflict`를 해결한 뒤 `merge`합니다.
- `merge` 완료된 브랜치는 바로 삭제합니다.

### 2. 면접 진행

1. 순서를 정하고, 돌아가며 준비한 질문을 받고 답변합니다.
2. 희망자만 캠을 켜고, 진행 시간은 인당 최대 30분으로 합니다.
3. 꼬리 질문은 질문자가 기록해 두었다가 답변자에게 공유합니다.
4. 피드백은 면접 중 기록해 두었다가 한 명의 면접이 끝나면 바로 진행합니다.
5. 모두의 면접이 종료되면 다음 회차의 주제를 선정합니다.

<br/>

## 🆕 질답 PR 생성 순서

### 1. 로컬 저장소 최신화

`origin`의 `main` 브랜치를 `pull` 해서 로컬 저장소의 `main` 브랜치를 최신화합니다.

```shell
git pull origin main
```

### 2. 브랜치 생성

브랜치 이름은 `일자/이름`으로 합니다. ex) `230823/chun-gu`

```bash
git checkout -b 230823/chun-gu
```

### 3. 질답 작성

- 주제에 맞는 폴더의 `README.md`에 질답을 작성합니다.  
  ex) `react/README.md`, `javascript/README.md`
- 복습을 한다는 생각으로 자신이 미리 준비했던 질문과 답변에 더해 자신이 받은 꼬리 질문과 답변도 작성하는데, 부족했던 부분을 보완 및 보충합니다.

### 4. commit & push

작성한 질답을 `commit` 하고 개인의 원격 저장소(`origin`)에 `push` 합니다.

```shell
git add .
git commit -m "doc: add chun-gu's qna on 230823"
git push origin 230823/chun-gu
```

### 5. PR 제출

다음 스터디 시작 전까지 organization 저장소의 `main` 브랜치로 `PR`을 제출합니다.
