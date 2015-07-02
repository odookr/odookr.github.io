# Odoo Translation


웹 번역 플랫폼으로 https://transifex.com/ 사용


## Translating with Transifex

Transifex 번역하기 위해 사용될 수있는 웹 기반의 변환 인터페이스를 제공한다. 주요 기능은 다음과 같습니다 
 - 웹 기반, 누구를위한 모든 곳에서 접근하고 개발자의 기술을 필요로하지 않는 자동으로 공동 검토 기능을 제공합니다.
 - 다른 버전 및 모듈에 존재하는 유사한 용어를 번역하는 번역 메모리를 제공하며, 하나는해야한다고 생각 조건을 표시 할 수 있습니다 : 
 - 자동으로 소스 코드 파일 (.pot)와 동기화 다른 번역가, 번역을 포함 Transifex에 등록한 후에게 수동 업데이트의 필요성을 제거하여 검토, 
 - 당신은 조직 페이지에 어떤 Odoo 프로젝트의 번역에 액세스 할 수 있습니다

### 언제 Odoo (혹은 Github)에 번역이 보여지나?
번역팀에서 리뷰어에 의해 리뷰가 되고, 허용된 번역물은 Odoo 소스에 담겨진다. 동기화 주기는:
 - 8.0: weekly (on Mondays)
 - 7.0: every two weeks (on Mondays)



## Transifex client 설치
virtualenv를 이용해 가상 개발환경을 만들고 그곳에서 transfex-client 를 설치해 사용한다.

### 가상 환경 설치
virtualenv를 기반으로 virtualenvwrapper로 개발 프로젝트 단위를 사용한다.
virtualenv 를 설치하고 virtualenvwrapper 환경을 만든다.

#### 프로젝트 생성

```bash
$workon          # 프로젝트 목록
$mkvirtualenv project_name
(project_name)$ 
```

그리고 pip로 설치할 수 있다.
```bash
(project_name)$ pip install transifex-client
```

### Odoo-8 번역 소스 다운받기

https://gist.githubusercontent.com/mart-e/19754ea3bd81e5d67644/raw/5a1e07f0ba466b33c2dd42864209c87a5c1234e3/config


다음 같이 프로젝트 폴더를 만들고 가상환경을 실행합니다. 그리고 tx 저장소를 초기화 합니다.
 - transifex 에 가입되야 하고 아이디와 비밀번호가 필요.

```bash
$ mkdir transifex-odoo8
(transifex)gogangtaeui-Air:Works thinkbee$ cd transifex-odoo8/
(transifex)gogangtaeui-Air:transifex-odoo8 thinkbee$ tx init
Creating .tx folder...
Please enter your transifex username:: gangtai.goh@gmail.com
Password:
Updating /Users/thinkbee/.transifexrc file...
Done.
(transifex)gogangtaeui-Air:transifex-odoo8 thinkbee$
```

이제 tx 명령을 이용해 저장소에 다운로드 합니다.

```
tx pull -r "odoo-8.*" -l ko --mode reviewed # review한 모든 ko 
```
tx pull -s -r "odoo-7.*" # get all source terms (.pot) for Odoo 7.0
tx push -s -r "odoo-8.crm" -l es # send my Spanish translation suggestions for the crm module for Odoo 8.0


