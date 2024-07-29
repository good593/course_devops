FROM python:3.12 AS python-build
RUN pip install mysqlclient

FROM python:3.12-slim
COPY --from=python-build /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
RUN apt-get update && apt-get install -y libmariadb3

# 작업 디렉토리 설정
WORKDIR /app

RUN mkdir -p /app/.streamlit

# 소스 복사 
COPY ex-01.py requirements.txt /app/
COPY .streamlit/secrets.toml /app/.streamlit/secrets.toml

# 라이브러리 설치 
RUN pip3 install --upgrade pip
RUN pip3 install -r requirements.txt

# 포트 오픈 
EXPOSE 8501

# 컨테이너 헬스 체크
HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

# 애플리케이션 실행 
ENTRYPOINT ["streamlit", "run", "ex-01.py", "--server.port=8501", "--server.address=0.0.0.0"]
