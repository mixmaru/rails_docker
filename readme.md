# これは何？

railsの開発サーバーでの開発と、
dockerを使ってproduction環境での確認ができる環境を構築する設定ファイル郡

# バージョン

ruby 2.6.3
rails 6

# 構築方法

## railsの新規インストール
```
git clone https://github.com/mixmaru/rails_docker.git
cd rails_docker/app
rbenv local 2.6.5
bundle install --path vendor/bundle
bundle exec rails new . （Gemfileを上書き聞かれるのでnを入力）
```

## dbの起動と設定
```
cd rails_docker/
```
dbの起動
```
docker-compose up -d db
```

railsのdb接続情報ファイルを編集
rails_docker/app/config/database.yaml
```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5
  username: db_user
  password: password

development:
  <<: *default
  host: localhost
  port: 5432
  database: rails_db_dev


test:
  <<: *default
  host: localhost
  port: 5432
  database: rails_db_test


production:
  <<: *default
  host: db
  port: 5432
  database: rails_db
```

開発サーバーの起動
```
bundle exec rails s 
```

localhost:3000でアクセスで表示されるはず

## dockerでproductionで動かす
```
cd rails_docker/
docker-compose up -d
```

localhost:80でアクセスで表示されるはず
