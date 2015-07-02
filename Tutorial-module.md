# 모듈 구성하기

> Warning
> 이 튜로티렁릉 Odoo가 설치되어 있다고 가정합니다.


## Start/Stop the Odoo server

서버를 실행하기 위해서 간단히 odoo.py 를 실행하면 됩니다. 필요에 따라 절대경로를 주어야 합니다.
```bash
$ odoo.py
```

시작한 터미널에서 Ctrl+C 를 두번 입력하면 서버 실행을 종료할 수 있습니다.


## Odoo 모듈 구성하기
서버와 클라이언트 확장은 모두 모듈로, 선택적으로 데이터베이스에 적재되어 묶여 있습니다.

Odoo 모듈들은 새로운 비즈니스 로직을 오두시스템에 추가하거나 교환할 수 있고 기존의 로직을 확대할 수 있습니다. 예를들어 해당 국가의 회계 규정을 오두의 일반 회계 지원에 추가할 수 잇습니다.

Odoo 안의 모든 것은 모듈로 시작하고 모듈로 종료를 합니다.

### Composition of a module
Odoo 모듈은 여러 요소를 포함하고 있습니다:
 - Business objects
   파이썬 클래스로 선언되어 설정에 따라 자원들은 자동으로 Odoo 기반으로 고정된다.
 - Data files
   XML, CSV 파일들은 
   XML or CSV files declaring metadata (views or workflows), configuration data (modules parameterization), demonstration data and more
 - Web controllers
   Handle requests from web browsers
 - Static web data
   Images, CSS or javascript files used by the web interface or website


### Module structure


# 참조

- [Building a Module](https://www.odoo.com/documentation/8.0/howtos/backend.html)

