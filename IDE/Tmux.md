---
title: Tmux 명령어
---

tmux : terminal multiplexer

## Session(호스트 쉘에서)

조회/생성
- 세션 리스트 확인 : `tmux ls`
- 세션 생성 : `tmux new -s 세션명`
- 세션과 윈도우 생성 : `tmux new -s 세션명 -n 윈도우명`
- 세션 다시 접속 : `tmux a -t 세션명`
- tmux 종료 : `tmux kill-server`
- 세션 종료 : `tmux kill-session -t 세션명`
- 윈도우 종료 : `tmux kill-window -t 윈도우명`

## Session(세션 내에서)

삭제
- 세션 나가기 (세션 유지) : `^b` + `d` *(detach)*
- 세션 나가기 (세션 중단) : `exit` or `^d`

수정
- 세션명 수정 : `^b` + `$`

## Window

생성/삭제
- 윈도우 생성 : `^b` + `c`
- 윈도우명 수정 : `^b` + `,`
- 윈도우 종료 : `^d` or `^b` + `&`

이동
- 윈도우 이동(번호지정) : `^b` + `윈도우번호`
- 윈도우 이동(오른쪽) : `^b` + `n` *(next)*
- 윈도우 이동(왼쪽) : `^b` + `p` *(previous)*
- 윈도우 이동(바로이전) : `^b` + `l` *(last)*
- 윈도우 이동(선택) : `^b` + `w`
- 윈도우 이동(찾기) : `^b` + `f` *(find)*

## Pane

생성/삭제
- 틀 오른쪽에 생성 : `^b` + `%` 
- 틀 아래에 생성 : `^b` + `"`
- 틀 삭제 : `^d` or `^b` + `x`

커서 이동
- 틀 이동(번호지정) : `^b` + `q` + `틀번호`
- 틀 이동(다음) : `^b` + `o`
- 틀 이동(화살표) : `^b` + `화살표`

틀 이동
- 틀 이동(왼쪽) : `^b` + `{`
- 틀 이동(오른쪽) : `^b` + `}`

변경
- 틀 사이즈 조정 : `^b` + `alt+방향키`
- 틀 레이아웃 변경(자동) : `^b` + `스페이스바`
- 틀 전체화면/부분화면(토글) : `^b` + `z`

## 기타

- help : `^b` + `?`


## 설정

`~/.tmux/conf`파일 설정 내용

```
# 스크롤 버퍼 사이즈 변경
set -g history-limit 200000

# Window 이동을 Shift + 왼쪽/오른쪽으로 변경
bind -n S-Left  previous-window
bind -n S-Right next-window

# Pane 이동을 Alt + 화살표로 변경
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
```

- 설정 reload시 : `^b` + `:`로 커맨드로 진입 후
```
:source-file ~/.tmux.conf
```
