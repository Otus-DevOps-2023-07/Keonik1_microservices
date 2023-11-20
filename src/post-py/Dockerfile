FROM python:3.6-alpine
# ARG http_proxy=http://172.17.0.1:3128
# ARG https_proxy=http://172.17.0.1:3128
WORKDIR /app
ADD . /app

RUN apk --no-cache --update add build-base && \
    pip install -r /app/requirements.txt && \
    apk del build-base

ENV POST_DATABASE_HOST post_db
ENV POST_DATABASE posts

ENTRYPOINT ["python3", "post_app.py"]
