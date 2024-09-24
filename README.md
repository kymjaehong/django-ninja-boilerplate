1. 가상 환경 설정
```
(Python 3.11.6, 3.12.1, ...)
python -m venv venv

 .\venv\Scripts\activate (윈도우)
 source .\venv\bin\activate (맥)
 
 pip install -r requirements-dev.txt
 
 (Git 연결 시)
 pre-commit install (ruff linter & formatter)
 
 (파이참 사용 시)
 Add New Interpreter > Add Local Interpreter > Existing
 Ruff plugin 설치
```
2. Django 프로젝트 설정
```
cd src
django-admin startproject shared .
```
3. Docker DB 사용 시
```
docker-compose -f .\docker-compose.{DB 명}.local.yml up --build -d
```
4. env 설정 (필요 시, 다른 설정 추가)
```
PRINT_SQL=1

shared.settings.py 상단에 load_dotenv() 호출
```
5. shared.settings.py 수정
```
# Docker DB 사용 시, EXPOSE PORT 변경 사용

# Postgres
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql_psycopg2",
        "NAME": "boilerplate",
        "USER": "root",
        "PASSWORD": "1234",
        "HOST": "127.0.0.1",
        "PORT": "5433",
    }
}
# MySQL (도커 파일 설정이 Postgres와 차이가 있으니 주의)
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": "boilerplate",
        "USER": "user",
        "PASSWORD": "1234",
        "HOST": "localhost",
        "PORT": "3307",
    }
}

if os.getenv("PRINT_SQL"):
    LOGGING = {
        "version": 1,
        "disable_existing_loggers": False,
        "handlers": {
            "console": {
                "class": "logging.StreamHandler",
            },
        },
        "loggers": {
            "django.db.backends": {
                "handlers": ["console"],
                "level": "DEBUG",
            },
        },
    }

```
6. 서버 실행
```
python .\manage.py runserver
```