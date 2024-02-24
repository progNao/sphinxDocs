Springbootの開発環境構築
==================================

概要
----------------------------------
* DockerでSpringbootの環境を構築する際の手順
* PostgreSQLも同時に作成し起動する
* projectはSpringInitializerで雛形を作成する

フォルダとファイルを作成する
----------------------------------
.. code-block::

   mkdir app
   touch docker-compose.yml
   mkdir initdb
   cd initdb
   touch init.sql

docker-compose.ymlを記述する
----------------------------------
.. code-block::
   :linenos:
    
   version: '3'
   services:
   spring:
      image: openjdk:15
      ports:
         - 8080:8080
      tty: true
      volumes:
         - ./app:/srv:cached
      working_dir: /srv
      depends_on:
         - db

   db:
      image: postgres:13.1
      environment:
         POSTGRES_USER: user
         POSTGRES_PASSWORD: pass
      ports:
         - "5432:5432"
      volumes:
         - dbvol:/var/lib/postgresql/data
         - ./initdb:/docker-entrypoint-initdb.d

   volumes:
   dbvol:

.. note::
   * serviceでdbとspringコンテナを命名
   * springではopenjdkを指定
   * volumesでローカルファイルをコンテナにマウントする
   * ポートはデフォルトの8080
   * depends_onでdbを接続
   * dbのimageはpostgresqlを選択
   * ポートはpostgresqlのデフォルトの5432
   * volumesでデータの永続化と初期化SQLを指定（下記の永続化用コンテナを参照）
   * environmentでpostgresqlの情報を指定
   * volumesでdbvolというコンテナ名でデータを永続化

SpringInitializerでプロジェクト作成
----------------------------------------
* https://start.spring.io/から作成
* 作成されたzipファイルを、appフォルダに展開

コンテナのビルド
----------------------------------
.. code-block::

   docker-compose build

コンテナの起動
----------------------------------
.. code-block::

   docker-compose up -d

プロジェクトのビルド
----------------------------------
* コンテナに入り以下コマンドを実行
* これでプロジェクトがビルドされる

.. code-block::

   sh gradlew build

.. warning::
   * カレントディレクトリがsrvであることに注意する

jarを実行してアプリケーションを起動
-----------------------------------------
.. code-block::

   java -jar build/libs/sample-0.0.1-SNAPSHOT.jar

コンテナの終了
----------------------------------
.. code-block::

   docker-compose down

再ビルドを行う
----------------------------------
.. code-block::
   
   docker-compose build
