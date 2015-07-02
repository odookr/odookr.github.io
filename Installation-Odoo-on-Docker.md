# Odoo on Docker
 - https://github.com/odoo/docker-docs/tree/master/odoo

## Start a PostgreSQL server
```bash
$docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --name postgres postgres
```

만약 pgAdmin 같은 도구를 사용하려면 외부에서 postgres 에 접속이 되야 하는데, 위 명령은 외부에 postgres 포트가 제공되지 않는다. 그래서  port를 열고 시작하려면 '-P' 포트 옵션을 주어서 사용한다. 
```bash
$ docker run -d -P -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --name db postgres
```

 - -d : 컨테이너를 데몬 모드로 실행
 - -P: 컨테이너에서 사용하는 포트를 Host에 드러 내놓는다. 즉, 클라이언트에서 Docker Host안의 컨테이너 포트로 접근이 가능하게 된다.
 - --name postgres : postgres 란 컨테이너 이름

docker에서 실행한 Postgresql의 클라이언트를 사용하기 위해서 데이터베이스 컨테이너의 bash 쉘을 사용할 수 있습니다.
```
docker exec -it [CONTAINER_NAME OR ID] bash
```

앞서 실행한 'postgresql' 컨테이너에 로그인해 psql을 사용해 데이터베이스에 접속하는 것을 살펴보겠습니다. 
```
~$ docker exec -it postgres bash
root@429ad411149a:/#
root@429ad411149a:/# psql -U odoo                 #odoo role로 로그인
```

기본 admin 계정의 패스워드를 설정해 보겠습니다.
```
root@429ad411149a:/# UPDATE res_users SET password='new_password' WHERE login = 'admin';
UPDATE 1
```

docker에서 실행중인 컨테이너에 접근할 수 있는 포트는 'docker port CONTAINER'로 확인할 수 있습니다. 예를 들어 위에 실행한 컨테이너 'postgres' 포트는 다음 같이 확인할 수 있습니다:
```
$ docker port postgres
5432/tcp -> 0.0.0.0:49153
```



## Start an Odoo instance

```
$docker run -p 0.0.0.0:8069:8069 --name odoo --link db:postgres -t odoo
```

odoo 컨테이너를 재시작하려면 컨테이너를 멈추고 시작하면 됩니다.
```
$docker stop odoo
$docker start -a odoo
```

## Odoo 소스에서 시작하기
데이터베이스를 도커 컨테이너로 사용하고 Odoo는 소스에서 시작한다고 할 때를 생각해 보겠습니다. 앞서와 같이 Postressql을 도커 컨테이너로 실행하고 있다고 가정합니다.

### Odoo 소스
github에서 git clone https://github.com/odoo/odoo.git를 fork 해서 사용하는 것을 권합니다. 

```
$git clone https://github.com/odoo/odoo.git 
```

다음과 같은 항목이 설치가 완료되어 있습니다.

- Python 2.7
    - Pip를 이용해서 Odoo 소스에 필요한 모듈을 설치합니
    - '$pip install -r requirements.txt'
- Node.JS 설치; less 모듈
    - sudp npm install -g less less-plugin-clean-css

> 여기서 데이터베이스는 도커 이미지로 실행중이라고 가정합니다.

### Odoo 실행
odoo가 실행하려면 PostgreSQL Host 주소, 포트, user 와 password가 필요합니다. 설정파일 혹은 명령라인 옵션이 없다면 계정 사용자 아이디와 패스워드 없이 유닉스 5432 소켓 포트를 통해 접속을 시도합니다. (단 윈도우는 소켓이 아니다.)
다음 같이 윈도우와 리눅스/맥에서 기본 실행을 합니다.

#### 윈도우에서 실행

```
C:\YourOdooPath> python odoo.py -w odoo -r odoo --addons-path=addons,../mymodules --db-filter=mydb$
```
 - -w, -r은 Postgresql 데이터베이스 계정
 - 'mymodules' 사용자 작성 모듈
 - 기본 localhost:8069 에 접속

#### 리눅스/맥에서 실행

```
$ ./odoo.py --addons-path=addons,../mymodules
```

설정을 통해서 Postgresql 서버를 지정할 수 있습니다.

#### 데이터베이스 설정
odoo.py의 명령라인에 다음 설정을 이용해서 데이터베이스 호스트와 포트를 지정할 수 있습니다.
 - -r <db user> : 데이터베이스 계정 예) odoo
 - --db_host <hostname> : host for the database server
 - --db_port <port>: port the database listens on, defaults to 5432

```bash
$ ./odoo.py --addons=addons/,../odoomodules -r odoo -w odoo --db_host 192.168.59.103 --db_port 49153
2015-05-04 04:04:12,518 12256 INFO ? openerp: OpenERP version 8.0
2015-05-04 04:04:12,518 12256 INFO ? openerp: addons paths: ['/Users/thinkbee/Library/Application Support/Odoo/addons/8.0', u'/Users/thinkbee/odoo/addons', u'/Users/thinkbee/odoo-modules', '/Users/thinkbee/odoo/openerp/addons']
2015-05-04 04:04:12,518 12256 INFO ? openerp: database hostname: 192.168.59.103
2015-05-04 04:04:12,518 12256 INFO ? openerp: database port: 49153
2015-05-04 04:04:12,518 12256 INFO ? openerp: database user: odoo
2015-05-04 04:04:12,802 12256 INFO ? openerp.service.server: HTTP service (werkzeug) running on 0.0.0.0:8069
```

개발을 위해서 background로 odoo 를 실행하고자 하면 nohup 을 이용할 수 있다.
```
$ nohub odoo/odoo.py --addons=addons/,../odoomodules -r odoo -w odoo --db_host 192.168.59.103 --db_port 49153
...

$ ps -ef |grep odoo
its      19705     1  8 17:35 pts/0    00:00:00 python odoo/odoo.py -r odoo -w odoo --db_host localhost --db_port 49153
```


#### 설정 파일
설정 파일은 %PROGRAMFILES%\Odoo 8.0-id\server\openerp-server.conf 에서 찾을 수 있다.




### 유닉스 시그널
 UNIX /usr/include/sys/signal.h에서 확인하고 Linux는 /usr/include/bits/signum.h 참고)
혹은 직관적으로 확인하고자 한다면 프롬프트창에서 kill -l 을 치면 시스템 Signal 종류에 대해서 확인 할 수 있다.

기본적인 Signal과 은 아래와 같다. (시스템 마다 종류나 숫자는 차이가 있음)


1. SIGHUP: 연결된terminal이hangup하였을때(terminate)
2. SIGINT: interrupt key(^C)를입력하였을때(terminate)
3. SIGQUIT: quit key(^\)를입력하였을때(terminate+core)
4. SIGILL: illegal instruction을수행하였을때(terminate+core)
5. SIGTRAP: implementation defined hardware fault (terminate+core)
6. SIGABRT: abort시스템호출을불렀을때(terminate+core)
7. SIGBUS: implementation defined hardware fault (terminate+core)
8. SIGFPE: arithmetic exception, /0, floating-point overflow (terminate+core)
9. SIGKILL: process를kill하기위핚signal, catch 혹은ignore될수없는signal임(terminate)
10. SIGUSR1: user defined signal 1 (terminate)
11. SIGSEGV: invalid memory reference (terminate+core)
12. SIGUSR2: user defined signal 2 (terminate)
13. SIGPIPE: reader가terminate된pipe에write핚경우발생(terminate)
14. SIGALRM: alarm시스템호출후timer가expire된경우(terminate)
15. SIGTERM: kill시스템호출이보내는software termination signal (terminate)
16. SIGCHLD: child가stop or exit되었을때parent에게전달되는신호(ignore)
17. SIGCONT: continue a stopped process (continue/ignore)
18. SIGSTOP: sendable stop signal, cannot be caught or ignored (stop process)
19. SIGTSTP: stop key(^Z)를입력하였을때(stop process)
20. SIGTTIN: background process가control tty로부터read핛경우(stop process)
21. SIGTTOU: background process가control tty로write핛경우(stop process)
22. SIGURG: urgent condition on IO, socket의OOB data (ignore)
23. SIGXCPU: exceeded CPU time limit (terminate+core/ignore)
24. SIGXFSZ: exceeded file size limit (terminate+core/ignore)
25. SIGVTALRM: virtual time alarm, setitimer, (terminate)
26. SIGPROF: profiling time alarm, setitimer, (terminate)
27. SIGWINCH: terminal window size changed, (ignore)
28. SIGIO: 어떤fd에서asynchronous I/O event가발생하였을경우(terminate/ignore)
29. SIGPWR: system power fail (terminate/ignore)
30. SIGSYS: bad argument to system call (terminate+core)


