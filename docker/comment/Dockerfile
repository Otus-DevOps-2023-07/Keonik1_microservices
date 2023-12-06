FROM ruby:2.2
# ARG http_proxy=http://172.17.0.1:3128
# ARG https_proxy=http://172.17.0.1:3128
RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list && \
    apt-get update \
    && apt-get install -y build-essential \
    && rm -rf /var/lib/apt/lists/*

ENV APP_HOME /app
WORKDIR $APP_HOME

ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME

ENV COMMENT_DATABASE_HOST comment_db
ENV COMMENT_DATABASE comments

CMD ["puma"]
