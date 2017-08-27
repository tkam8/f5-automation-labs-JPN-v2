.. |labmodule| replace:: 2
.. |labnum| replace:: 1
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: ``f5-postman-workflows`` フレームワークのインストール
------------------------------------------------------------------------

このラボでは、 ``f5-postman-workflows`` フレームワークを ``Postman REST Client`` にインストールします。

Task 1 - Postman  ``Collections`` への ``f5-postman-workflows`` インポート
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

このタスクでは、インストールヘルパー関数、サンプル、自動テストフレームワークを含むPostman Collectionをインポートします。このコレクションは、 ``f5-postman-workflows`` のGitHubリポジトリからインストールされます。

このタスクを完了するには、次の手順を実行します:

#. ジャンパポストで、　|image8| アイコンをクリックし、Postmanクライアントを開きます。
#. Postman画面の左上にある ``Import`` ボタンをクリックします。
#. ``Import from Link`` タブをクリックします。テキストボックスに次のURLを貼り付け、 ``Import`` をクリックします。

   ``https://raw.githubusercontent.com/0xHiteshPatel/f5-postman-workflows/master/F5_Postman_Workflows.postman_collection.json``

#. Postman  ``Collections`` のサイドバーに ``F5_Postman_Workflows`` という名前のCollectionが表示されます。

Task 2 - Postmanクラインとに ``f5-postman-workflows`` フレームワークをインストール
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

フレームワークに含まれるヘルパー関数を利用するには、これらの関数をPostmanクライアントにインストールする必要があります。インストールヘルパは、次の操作を実行します。

#. フレームワークの最新バージョンを決定
#. GoogleのClosure Compilerを使用して、 ``f5-postman-workflows`` GitHubリポジトリからのJavaScriptコードを圧縮・最適化
#. 圧縮されたJavaScriptコードをPostmanグローバル変数にインストール
#. さまざまなオプションを設定できるグローバル変数を設定

フレームワークをインストールするには、次の手順を実行します:

#. ``F5_Postman_Workflows`` collectionを展開します。
#. ``Install`` フォルダを開きます。
#. ``Check f5-postman-workflows Version`` の項目を選択し、 ``Send`` をクリックします。
#. **RESPONSE**　の ``Tests`` の部分を確認します。

   |image79|

#. ``Install/Upgrade f5-postman-workflows`` の項目を選択し、 ``Send`` をクリックします。
#. ``Tests`` を再度確認し、 インストールが成功したことを確認します。

   |image80|

#. Postman画面の右上にある ``目`` ボタンをクリックし、 入力されたグローバル変数を確認します。

   |image81|

以上で、　``f5-postman-workflows``　フレームワークがPostmanクライアントにインストールされました。

.. |image8| image:: /_static/image008.png
   :width: 0.46171in
   :height: 0.43269in

.. |image79| image:: /_static/image079.png
   :scale: 100%

.. |image80| image:: /_static/image080.png
   :scale: 100%

.. |image81| image:: /_static/image081.png
   :scale: 100%
