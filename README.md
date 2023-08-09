### 手順

#### 1.git clone https://github.com/tsubasahirakida/docker_react_rails_dev

#### 2.Docker Desktop立ち上げ

#### 3.cd docker_react_rails_dev

#### 4.docker-compose build

#### 5.docker-compose run --rm front sh -c 'npx create-react-app front --template typescript'
→ y押す

#### 6.docker-compose.ymlの修正

修正前
```
front:
    build:
      context: ./
      dockerfile: Dockerfile
```

修正後
```
front:
    build:
      context: ./front/
      dockerfile: Dockerfile
```

#### 7.mv Dockerfile front/

#### 8.docker-compose run --rm api bundle exec rails new . --api -d mysql
→ y押す

#### 9.database.yml書き換え

修正後
```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password ←
  host: db ←

development:
  <<: *default
  database: typing ←
```

#### 10.docker-compose up -d --build

#### 11.docker-compose down

#### 12.再度コンテナを起動する際は、docker-compose up -d
※Dockerイメージは5の手順で作成されているため、再度コンテナ起動する際は、--buildオプション不要

#### 13.このアプリのDockerイメージも不要になれば、Dockerイメージ削除
```
$ docker images
$ docker rmi <IMAGE ID>
```
