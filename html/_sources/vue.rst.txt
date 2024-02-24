Vueの開発環境構築
==================================

概要
----------------------------------
* DockerでVueの環境を構築する際の手順

フォルダとファイルを作成する
----------------------------------
.. code-block::

   mkdir app
   touch Dockerfile
   touch docker-compose.yml

Dockerfileを記述する
----------------------------------
.. code-block::
   :linenos:
    
   FROM node:16.15.0
   WORKDIR /usr/src/app

.. note::
   * イメージはnodejsを選択する

docker-compose.ymlを記述する
----------------------------------
.. code-block::
   :linenos:
    
   version: '3'
   services:
   vue:
      build:
         context: .
         dockerfile: Dockerfile
      tty: true
      environment:
         - NODE_ENV=development
      volumes:
        - ./app:/usr/src/app
      command: sh -c "cd vue-app && npm run serve"
      ports:
        - "8080:8080"

.. note::
   * serviceでvueコンテナを命名
   * environmentでnodejsの環境設定
   * commandでコンテナ起動時のコマンドを指定
   * volumesでローカルファイルをコンテナにマウントする
   * ポートはデフォルトの8000

コンテナのビルド
----------------------------------
.. code-block::

   docker-compose build

Vueプロジェクトの作成
----------------------------------
.. code-block::

   docker-compose run --rm vue sh -c  "npm install -g @vue/cli @vue/cli-init && vue create vue-app"

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
