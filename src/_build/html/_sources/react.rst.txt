Reactの開発環境構築
==================================

概要
----------------------------------
* DockerでReactの環境を構築する際の手順

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
    
   FROM node:lts
   WORKDIR /usr/src/app

.. note::
   * イメージはnodejsを選択する

docker-compose.ymlを記述する
----------------------------------
.. code-block::
   :linenos:
    
   version: "3"
   services:
   react:
      build:
         context: .
         dockerfile: Dockerfile
      tty: true
      environment:
         - NODE_ENV=development
      volumes:
        - ./app:/usr/src/app
      command: sh -c "cd reactapp && yarn start" 
      ports:
        - 3000:3000

.. note::
   * serviceでreactコンテナを命名
   * environmentでnodejsの環境設定
   * commandでコンテナ起動時のコマンドを指定
   * volumesでローカルファイルをコンテナにマウントする
   * ポートはデフォルトの3000

コンテナのビルド
----------------------------------
.. code-block::

   docker-compose build

Reactプロジェクトの作成
----------------------------------
.. code-block::

   docker-compose run --rm react sh -c  "npx create-react-app react-app --template typescript"

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
