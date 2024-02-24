Djangoの開発環境構築
==================================

概要
----------------------------------
* DockerでDjangoの環境を構築する際の手順
* PostgreSQLも同時に作成し起動する
* requirements.txtで必要なモジュールを記述

フォルダとファイルを作成する
----------------------------------
.. code-block::

   mkdir code
   touch Dockerfile
   touch docker-compose.yml

Dockerfileを記述する
----------------------------------
.. code-block::
   :linenos:
    
   FROM python:3
   ENV PYTHONUNBUFFERED 1
   RUN mkdir /code
   WORKDIR /code
   COPY requirements.txt /code/
   RUN pip install -r requirements.txt
   COPY . /code/

.. note::
   * イメージはpython3を選択する
   * COPYでrequirements.txtとカレントディレクトリファイルをコピーする

docker-compose.ymlを記述する
----------------------------------
.. code-block::
   :linenos:
    
   version: '3'
   services:
   db:
      image: postgres
      ports:
         - "5432:5432"
      volumes:
         - db-data:/var/lib/postgresql/data
         - ./init:/docker-entrypoint-initdb.d
      environment:
         POSTGRES_USER: user
         POSTGRES_PASSWORD: pass
   web:
      build: .
      command: python manage.py runserver 0.0.0.0:8000
      volumes:
         - .:/code
      ports:
         - "8000:8000"
      depends_on:
         - db
   volumes:
   db-data:
      driver: local

.. note::
   * serviceでdbとwebコンテナを命名
   * dbのimageはpostgresqlを選択
   * ポートはpostgresqlのデフォルトの5432
   * volumesでデータの永続化と初期化SQLを指定（下記の永続化用コンテナを参照）
   * environmentでpostgresqlの情報を指定
   * webではDockerfileを読み込む
   * commandでコンテナ起動時のコマンドを指定（Djangoを実行するコマンド）
   * volumesでローカルファイルをコンテナにマウントする
   * ポートはデフォルトの8000
   * depends_onでdbを接続
   * volumesでdb-dataというコンテナ名でデータを永続化

コンテナのビルド
----------------------------------
.. code-block::

   docker-compose build

コンテナの起動
----------------------------------
.. code-block::

   docker-compose up -d

コンテナの終了
----------------------------------
.. code-block::

   docker-compose down

再ビルドを行う
----------------------------------
.. code-block::
   
   docker-compose build

.. warning::
   * requirements.txtなどを更新した場合
