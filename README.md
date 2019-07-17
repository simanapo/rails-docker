Ruby on Railsの開発環境をDockerで構築


■事前準備(中身はソースコード参照)

・Dockerfile

・docker-compose.yml

・Gemfile

・Gemfile.lock



■手順
・上記ファイルを用意したら、Railsプロジェクトを作成する

$ docker-compose run web rails new . --force --database=mysql --skip-bundle


・database.ymlを修正する
$ vi database.yml

default: &default

  adapter: mysql2
  
  encoding: utf8
  
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  
  username: root
  
  password: password # docker-compose.ymlのMYSQL_ROOT_PASSWORD
  
  host: db # docker-compose.ymlのservice名
  

・コンテナをビルド

$ docker-compose build


・DB作成

$ docker-compose run web rails db:create


・コンテナ起動

$ docker-compose up -d


・ブラウザにアクセス

http://localhost:3000/



■その他

・サーバーを止める

$ docker-compose down

Ctrl+Cで止めると、コンテナが残って次回起動時にエラーが出る

もしやっちゃったら、tmp/pids/server.pidを削除、再起動

再起動はdocker-compose upでできる


・Dockerfileやdocker-compose.ymlの変更を反映、railsサーバーを再起動

$ docker-compose up --build


・bundle installなどのコマンドを実行したい

#docker-compose run {サービス名} {任意のコマンド}

$ docker-compose run web bundle install


・ローカルからMySQLコンテナに接続

$ mysql -u root -p -h localhost -P 3306 --protocol=tcp



参考：https://qiita.com/azul915/items/5b7063cbc80192343fc0
