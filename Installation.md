# Installation
Odoo는 파이썬 2.7을 이용해 개발되어 있다.

## Source Installation
깃헙 odoo에서 포크한 다음 링크에서 브랜치를 가지고 작업한다.
 - https://github.com/qkboo/odoo


### Ubuntu Linux 
현재 Ubuntu 14.04 를 이용해 테스트해보았다.

#### 개발자 패키지 설치
Python, Postgresql, libxml2, libxslt, libevent와 libsasl2의 개발도구와 네이티브 의존 패키지인 -dev와 -devel 패키지를 설치해야 한다. 개발자 패키지가 설치되어 있어야 한다.

```
$sudo apt-get update
$sudo apt-get upgrade
$sudo apt-get install python-dev libpq-dev libxml2-dev libxslt1-dev libevent-dev libsasl2-dev libldap2-dev
```


#### Python 2.7.9 설치
여기서는 우분투에 설치되어 있는 파이썬 2.7.6을 이용한다.

#### PosgresSQL 설치
Postgresql 를 설치한 후에 기본 사용자 'postgres'가 생성되는데 Odoo에서 이 계정 사용은 지양한다.
 - docker에서 postgresql 은 설치한다.

# aptitude install postgresql
# aptitude install python-virtualenv
Create a system user and a PostgreSQL user :

# adduser odoo
# su postgres
$ createuser -dSR odoo



배포본이 제공하는 postgresql을 사용하고, 다음 같이 postgres 계정으로 사용자 계정을 사용한다:

```
$ sudo su - postgres -c "createuser -s $USER"
```
Because the role login is the same as your unix login unix sockets can be use without a password.


#### requirement.txt
apt 패키지 관리자로 설치할 수도 있다. 여기서 ** pip ** 를 사용한다.
네이티브 코드를 사용하는 라이브러리들(Pillow, lxml, greenlet, gevent, psycopg2, ldap)는  그리고나서 파이썬 의존성이 설치될 수 있다:

```
$ pip install -r requirements.txt
```



### Mac OS X
리눅스와 같은 postgres 사용자를 생성한다.

#### Python 2.7.9 설치
여기서는 리눅스, 맥에서 파이썬 2.7.9이 설치되었어야 한다.


### Windows
 - 설치 경로를 PATH 환경변수에 넣어 준다.
 - pg admin GUI를 이용해서 postgres 계정을 만든다. 예) odoo/odoo
 - 이 계정과 비밀번호를 -w와 -r 로 odoo에 전달하거나 설정 파일에 제공해야 한다.

### 모듈 의존성 파일 작성; requirements.txt


#### Mac OS X
Xcode Command Line Tools (```xcode-select --install```)을 설치해야 한다. 또한 Homebrew, Macports를 이용해 파이썬 이외의 개발 도구와 라이브러리를 설치한다.

```
$ pip install -r requirements.txt
```


$ sudo pip install simplejson



#### Windows
Windows you need to install some of the dependencies manually, tweak the requirements.txt file, then run pip to install the remaning ones.

Install psycopg using the installer here http://www.stickpeople.com/projects/python/win-psycopg/

Then edit the requirements.txt file:

remove psycopg2 as you already have it.
remove the optional python-ldap, gevent and psutil because they require compilation.
add pypiwin32 because it’s needed under windows.
Then use pip to install the dependencies using the following command from a cmd.exe prompt (replace \YourOdooPath by the actual path where you downloaded Odoo):

C:\> cd \YourOdooPath
C:\YourOdooPath> C:\Python27\Scripts\pip.exe install -r requirements.txt



### NVM기반의 Node.JS 설치

#### nvm 설치
Node Version Manager를 이용해서 node.js를 이용해 본다.
```
$ curl https://raw.githubusercontent.com/creationix/nvm/v0.25.1/install.sh | bash
```

설치후 NVM 쉘 스크립트 실행을 위해서 로그아웃후 다시 로그인하고 사용하낟.

```
$nvm install v0.12     ### Node.JS v0.12 최신버전을 설치
...
$nvm ls
->    v0.12.2
```

이제 nvm을 이용하므로 global 설치시에도 sudo를 사용하지 않아도 된다.

##### Less CSS 설치
node.js를 이용하고 있다.

```
$ npm install -g less less-plugin-clean-css
```



## Odoo 실행
의존성 설정이 완료되었다면 'odoo.py'를 실해서 Odoo를 공개할 수 있다.

'[Configuration](https://www.odoo.com/documentation/8.0/reference/cmdline.html#reference-cmdline)'은 [명령라인 매개변수](https://www.odoo.com/documentation/8.0/reference/cmdline.html#reference-cmdline) 혹은 설정 파일을 통해서 제공할 수 있다.

필요한 공통 설정 요소는:
 - PostgreSQL host, port, user와 password.
   Odo has no defaults beyond psycopg2’s defaults: connects over a UNIX socket on port 5432 with the current user and no password. By default this should work on Linux and OS X, but it will not work on windows as it does not support UNIX sockets.
 - 사용자 보유 모듈을 적재하려면 애드온 경로를 제공해야 한다

### Windows
윈도우즈에서 일반적인 Odoo 실행 :

```
C:\YourOdooPath> python odoo.py -w odoo -r odoo --addons-path=addons,../mymodules --db-filter=mydb$
```
Where odoo, odoo are the postgresql login and password, ../mymodules a directory with additional addons and mydb the default db to serve on localhost:8069

### Unix
유닉스 계열에서 일반적인 Odoo 실행:

```
$ ./odoo.py --addons-path=addons,../mymodules -r odoo
```
 - **'.../mymodules'**는 애드온 디렉토리이고 **'mydb'**는 기본 호스트 'localhost:8069'에서 서비스하는 데이터베이스이다.

## Python 가상개발환경

$ virtualenv pyenv
$ source pyenv/bin/activate
$ pip install pip --upgrade
$ pip install setuptools --upgrade
$ pip install anybox.paster.odoo
$ paster create -t odoo_instance my_odoo
$ deactivate
After answering a few questions, you can bootstrap and run the buildout :

$ cd my_odoo
$ virtualenv --no-setuptools odooenv

#### 참조 [Installing Odoo 8 on Linux for development (Debian 7 or Ubuntu 14.04 and 14.10)](https://anybox.fr/blog/installing-odoo-8-on-linux-for-development-debian-ubuntu)



## Package 기반 설치

https://www.odoo.com/forum/help-1/question/how-to-install-odoo-from-github-on-ubuntu-14-04-for-testing-purposes-only-ie-not-for-production-52627



## 국제화 파일
사용하는 언어에 대한 번역 파일은 각 모듈의 ‘i18n’ 디렉토리에 국가_언어.po 파일로 저장되어 있다. 예를 들어 base 모듈의 파일을 살펴보면;

odoo/openerp/addons/base/i18n$ ls
ab.po     da.po     es_EC.po  fi.po     id.po  mn.po     sl.po        ur.po
af.po     de.po     es_HN.po  fr.po     is.po  nb.po     sq.po        vi.po
am.po     el.po     es_MX.po  fr_CA.po  it.po  nl.po     sr.po        zh_CN.po
ar.po     en_GB.po  es_PA.po  gl.po     ja.po  nl_BE.po  sr@latin.po  zh_HK.po
base.pot  es.po     es_PE.po  gu.po     ka.po  pl.po     sv.po        zh_TW.po
bg.po     es_AR.po  es_VE.po  he.po     kk.po  pt.po     ta.po
bn.po     es_BO.po  et.po     hi.po     ko.po  pt_BR.po  th.po
bs.po     es_CL.po  eu.po     hr.po     lt.po  ro.po     tlh.po
ca.po     es_CR.po  fa.po     hu.po     lv.po  ru.po     tr.po
cs.po     es_DO.po  fa_AF.po  hy.po     mk.po  sk.po     uk.po

Transfiex 등에서 애드온 번역 PO 파일을 다운받아 odoo/addons/{module}/i18n/{COUNTRY}.po 파일로 복사해 사용하면 된다.

기본 언어는 시스템의 기본 언어 설정에 따라서 모듈의 i18n 파일을 찾게 된다. 설치시 ‘한국어'를 선택했다면, Administrator -> 기본설정 메뉴를 찾아 보면 확인할 수 있다.
예) 한국어는 ko.po 파일로 저장한다.



## 참조
 - [Setup Install Source](https://www.odoo.com/documentation/8.0/setup/install.html#setup-install-source)
