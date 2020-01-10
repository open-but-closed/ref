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

### `conda create` 환경 생성하기

`conda env create`또는 `conda-env create`와 다르게 `conda create`로도 생성할 수 있음
기존 명령어에 추가적으로 설정할 수 있는 옵션이 많음

- `--copy` : 패키지를 소프트링크나 하드링크가 아닌 실제 복사(copy)를 해서 생성함
- `--clone` <환경이름> : <환경이름>을 복제한 환경을 생성함

```bash
$ conda create --clone 기존환경 -n 신규환경
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

- Windows에선 `activate`로 수행
- Mac이나 Linux에선 `source activate`로 수행

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

`conda activate 환경이름` 역시 가능하다

```bash
$ conda activate gcal_env_clone
(gcal_env_clone) $

```

### deactivate

마찬가지임

```bash
(gcal_env) $ source deactivate

$

```

또는

```bash
(gcal_env) $ conda deactivate

$

```


## (기타) 버전 옵션

- Fuzzy : `numpy=1.11` 1.11.0, 1.11.1, 1.11.2, 1.11.18 etc.
- Exact : `numpy==1.11` 1.11.0
- Greater than or equal to : `"numpy>=1.11"` 1.11.0 or higher
- OR : `"numpy=1.11.1|1.11.3"` 1.11.1, 1.11.3
- AND : `"numpy>=1.8,<2"` 1.8, 1.9, not 2.0

NOTE: Quotation marks must be used when your specification contains a space or any of these characters: > < | *


# 참조

- [CONDA CHEAT SHEET](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)