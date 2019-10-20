FROM ubuntu:18.04

WORKDIR /root

# ロケール設定(pipenvでPipfile.lockを作るときにエラーで怒られた為。railsでは必要か不明)
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8

# shellをbashに指定
SHELL ["/bin/bash", "-c"]

# 最新状態へ
RUN apt update
RUN apt -y upgrade

# 必要なコマンドのインストール
RUN apt -y install build-essential curl git zlib1g-dev libssl-dev libbz2-dev libreadline-dev libsqlite3-dev wget

# nginxのインストール
RUN apt -y install nginx

# cronのインストール
RUN apt -y install cron

# rubyのインストール
RUN wget https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.5.tar.gz && \
tar -zxvf ruby-2.6.5.tar.gz && \
cd ./ruby-2.6.5 && \
./configure && \
make install

# bundlerのインストール
RUN gem install bundler

# yarnのインストール（rails serverで必要）
# インストール参考https://yarnpkg.com/en/docs/install#debian-stable
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get -y install yarn

# コード転送
WORKDIR /app
ADD ./ ./
ADD ./rails_app.conf /etc/nginx/conf.d/rails_app.conf

# bundle install
# gem install pgで必要なのでインストール
RUN apt -y install libpq-dev
# rails dbconsoleで必要なのでインストール
RUN apt -y install postgresql-client
RUN bundle install --path vendor/bundle

# yarn install
RUN yarn install --check-files

EXPOSE 80

CMD /etc/init.d/cron start && bundle exec rails s -e production & nginx -g "daemon off;"
