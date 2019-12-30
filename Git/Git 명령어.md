# Git 명령어

## git config : 설정

### 파일 기록 대상

실제 아래 파일에 기록이 되며, 적용 시 `system` -> `global` -> `local` 식으로 덮어쓰면서 적용됨. 
- `--local` 설정 : `.git/config` 파일에 저장 - **기본값**
- `--global` 설정 : `~/.gitconfig` 파일에 저장
- `--system` 설정 : `/etc/gitconfig` 파일에 저장
- `--file <파일명>`, `-f <파일명>` : 특정 `<파일명>`에 설정값 저장

### 명령어

- `--list`, `-l` : 설정값 출력
- `--add <키> <값>` : `<키>`에 `<값>` 추가. `--add`는 생략 가능`
- `--unset <키>` : `<키>`에 대한 속성 제거

### 주요 `<키>`

- `user.name` : 사용자명
- `user.email` : 사용자 이메일


## git status : 상태확인

- git 현재 상태 확인하기

## git add : tracked -> staging area 

- `.`
- `*`

## git commit

- `-m <메시지>` : 커밋 메시지
- `-a` : `git add` 와 동일, tracked -> staging area로 올리기
