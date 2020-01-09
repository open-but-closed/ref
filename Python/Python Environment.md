# Python Environment 설정

## 아나콘다의 `conda-env` 이용하여 환경 생성/조회/삭제

- `-h`,`--help` : 사용법

- `list` : 목록보기
- `create` : 생성하기
- `remove` : 삭제하기
- `update` : 수정하기
- `export` : 내보내기

### `conda-env create` 환경 생성하기

- `-n` or `--name` <환경이름> : <환경이름>으로 생성
- `-f` or `--file` <환경파일> : export된 <환경파일>로 생성. 기본값은 `environment.yml`
- `-q` or `--quite` : 조용히
- `--force` : 기존 것 지우고 강제로

`<환경이름>`으로, python버전은 3.6으로 생성

```bash
$ conda-env create -n 환경이름 python=3.6
```

### `conda-env list` 환경 조회하기

조회하기

```bash
$ conda-env list
# conda environments:
#
base                  *  /Users/we/anaconda
gcal_env                 /Users/we/anaconda/envs/gcal_env
ifiw                     /Users/we/anaconda/envs/ifiw
py27                     /Users/we/anaconda/envs/py27
```

### `conda-env remove` 환경 삭제하기

- `-n` or `--name` <환경이름> : <환경이름> 삭제하기
- `-q` or `--quite` : 조용히
- `-y` or `--yes` : 예예

```bash
$ conda-env remove -n ifiw

Remove all packages in environment /Users/we/anaconda/envs/ifiw:

Proceed ([y]/n)? y
```

## 환경 activate/deactivate

`activate`와 `deactivate`는 모두 Conda의 쉘로 존재한다.

```bash
$ which activate deactivate
/Users/we/anaconda/bin/activate
/Users/we/anaconda/bin/deactivate
```

### activate

실제 사용에선 `activate`보다 `source activate`를 권장함

```bash
$ which python
/Users/we/anaconda/bin/python
# Python은 OS 위치에 있는 Python 참조

$ activate gcal_env
Error: activate must be sourced. Run 'source activate envname'
instead of 'activate envname'.
# source activate envname 형식으로 사용하라고...

$ source activate gcal_env

(gcal_env) $ which python
/Users/we/anaconda/envs/gcal_env/bin/python
# Python은 env의 Python을 참조

```

### deactivate

마찬가지임

```bash
(gcal_env) $ source deactivate

$

```
