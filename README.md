### 手順

#### 1.git clone https://github.com/tsubasahirakida/docker_react_rails_dev

#### 2.Docker Desktop立ち上げ

#### 3.cd docker_react_rails_dev

#### 4.docker-compose build

#### 5.docker-compose run --rm front sh -c 'npx create-react-app front --template typescript'
→ y押す

#### 6.front側のDockerfileの修正

package.jsonをDockerコンテナ側にマウントする必要があるので、
下記のCOPY以下の3行をfront側のDockerfileに追記

```
FROM node:18.17.0-alpine3.18
WORKDIR /usr/src/app

COPY package.json /usr/src/app
ENV PATH /usr/src/app/node_modules/.bin:$PATH
RUN npm install
```

#### 7.docker-compose.ymlの修正

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

#### 8.mv Dockerfile front/

#### 9.docker-compose run --rm api bundle exec rails new . --api -d mysql
→ y押す

#### 10.database.yml書き換え

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

#### 11.docker-compose up -d --build

#### 12.サーバーが立ち上がったか確認

rails:[localhost:3000  ](http://localhost:3000/)  
react:[localhost:8000  ](http://localhost:8000/)  

#### 13.docker-compose down

#### 14.再度コンテナを起動する際は、docker-compose up -d
※Dockerイメージは5の手順で作成されているため、再度コンテナ起動する際は、--buildオプション不要

#### 15.このアプリのDockerイメージも不要になれば、Dockerイメージ削除
```
$ docker images
$ docker rmi <IMAGE ID>
```
