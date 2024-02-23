Railsの開発環境構築
==================================

概要
----------------------------------
* UbuntuでRails7の環境を構築する際の手順
* rbenvでruby環境を作成
* gitはインストール済み

rbenvをインストール
----------------------------------
.. code-block::

   sudo apt install rbenv

ruby-buildをインストール
----------------------------------
.. code-block::

   git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

rubyをインストール
----------------------------------
.. code-block::

   rbenv install 3.x.x

.. warning::
   * インストールには時間がかかる

インストールされたことを確認
----------------------------------
.. code-block::

   ruby -v

rubyをグローバル化
----------------------------------
.. code-block::

   rbenv global 3.x.x

bundleがインストールされていることを確認
------------------------------------------
.. code-block::

   bundle -v

railsをインストール
----------------------------------
.. code-block::

    gem install rails

インストールされたことを確認
----------------------------------
.. code-block::

    rails -v

railsアプリケーションを作成
----------------------------------
.. code-block::

   rails new app
   cd app

Gemfileを更新してgemをまとめてインストール
-------------------------------------------
.. code-block::
   :linenos:

   gem "haml-rails"
   gem "sassc-rails"
   gem "font-awesome-rails"
   gem "devise"
   gem 'rails-i18n'

.. note::
   * 不要な場合や、違う場合は別途自分で記述

記述したgemをインストールする
----------------------------------
.. code-block::
   
   bundle install
   bundle update

railsを起動
----------------------------------
.. code-block::
   
   rails s
