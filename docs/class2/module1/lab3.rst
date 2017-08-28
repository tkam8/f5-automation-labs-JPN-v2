.. |labmodule| replace:: 1
.. |labnum| replace:: 3
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: 「f5-super-netops-container」コンテナへのアクセス
------------------------------------------------------------------

これまでのラボでは、コンテナイメージを起動し、ルートコマンドプロンプトが表示されました。コンテナとその関連ツールを使用するには、コンテナイメージに更にSSHまたはHTTPで接続します。

.. _lab1_3_1:

Task 1 – SSHでの接続
~~~~~~~~~~~~~~~~~~~~~~~~

SSH経由でイメージに接続するには、``docker run`` コマンドで指定された外部用ポート番号を使用する必要があります。前のタスクでは、コンテナを起動するためのコマンドは次のとおりでした。

``docker run -p 8080:80 -p 2222:22 -p 10000:8080 --rm -it -e SNOPS_GH_BRANCH=develop
f5devcentral/f5-super-netops-container:develop-jenkins``

これにより、SSHサービス ``TCP/22`` がDockerホスト上の ``TCP/2222`` に公開されます。また、SSHサービスの場合、次のマッピングが適用されます。

``localhost:2222 -> f5-super-netops-container:22``

.. NOTE:: F5提供のラボ環境を使用している場合は、SSHクライアントの ``f5-super-netops-container SSH`` に接続してください。

ログインする際に、ユーザ名 ``snops`` とパスワード ``default`` を入力してください。

コマンド例:

``ssh -p 2222 snops@localhost``

.. NOTE:: ホストのSSHキーは、コンテナが起動するたびに再生成されます。その結果、接続しようとしたときにホストキーが変更されたことを示すエラーが表示されることがあります。 このエラーは、``〜/ .ssh / known_hosts`` からキーを削除することで解決できます。また、``~/ .ssh / config`` に以下を追加することでローカルSSH設定を構成することもできます。

   .. code::

       Host localhost
       Port 2222
       StrictHostKeyChecking no
       UserKnownHostsFile /dev/null

出力例:

.. code::

   $ ssh -p 2222 snops@localhost
   Warning: Permanently added '[localhost]:2222' (ECDSA) to the list of known hosts.
   snops@localhost's password:
                                   .----------.
                                  /          /
                                 /   ______.'
                           _.._ /   /_
                         .' .._/      '''--.
                         | '  '___          `.
                       __| |__    `'.         |
                      |__   __|      )        |
                         | | ......-'        /
                         | | \          _..'`
                         | |  '------'''
                         | |                      _
                         |_|                     | |
    ___ _   _ _ __   ___ _ __          _ __   ___| |_ ___  _ __  ___
   / __| | | | '_ \ / _ \ '__| ______ | '_ \ / _ \ __/ _ \| '_ \/ __|
   \__ \ |_| | |_) |  __/ |   |______|| | | |  __/ || (_) | |_) \__ \
   |___/\__,_| .__/ \___|_|           |_| |_|\___|\__\___/| .__/|___/
             | |                                          | |
             |_|                                          |_|

   Welcome to the f5-super-netops-container.  This image has the following
   services running:

    SSH  tcp/22
    HTTP tcp/80

   To access these services you may need to remap ports on your host to the
   local container using the command:

    docker run -p 8080:80 -p 2222:22 -it f5devcentral/f5-super-netops-container:base

   From the HOST perspective, this results in:

    localhost:2222 -> f5-super-netops-container:22
    localhost:8080 -> f5-super-netops-container:80

   You can then connect using the following:

    HTTP: http://localhost:8080
    SSH:  ssh -p 2222 snops@localhost

   Default Credentials:

    snops/default
    root/default

   Go forth and automate!
   [snops@f5-super-netops] [~] $

Task 2 – HTTPでの接続
~~~~~~~~~~~~~~~~~~~~~~~~~

HTTP経由でイメージに接続するには、``docker run`` コマンドで指定された外部用ポート番号を使用する必要があります。前のタスクでは、コンテナを起動するためのコマンドは次のとおりでした。

``docker run -p 8080:80 -p 2222:22 -p 10000:8080 --rm -it -e SNOPS_GH_BRANCH=develop
f5devcentral/f5-super-netops-container:develop-jenkins``

これにより、HTTPサービス ``TCP/80`` がDockerホスト上の ``TCP/8080`` に公開されます。また、HTTPサービスの場合、次のマッピングが適用されます。

``localhost:8080 -> f5-super-netops-container:80``

.. NOTE:: F5提供のラボ環境を使用している場合は、Webブラウザで登録されている ``Super Netops Container`` お気に入りのサイトをクリックしてください。

HTTPで接続するには、Webブラウザを開き、次のURLを入力します。

``http://10.1.1.8:8080/start``

以下のような画面が表示されます。

|image78|

Task 3 – Jenkinsでの接続
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Jenkins経由でイメージに接続するには、``docker run`` コマンドで指定された外部用ポート番号を使用する必要があります。前のタスクでは、コンテナを起動するためのコマンドは次のとおりでした。

``docker run -p 8080:80 -p 2222:22 -p 10000:8080 --rm -it -e SNOPS_GH_BRANCH=develop
f5devcentral/f5-super-netops-container:develop-jenkins``

これにより、Jenkinsサービス ``TCP/8080`` がDockerホスト上の ``TCP/10000`` に公開されます。また、Jenkinsサービスの場合、次のマッピングが適用されます。

``10.1.1.8:10000 -> f5-super-netops-container:8080``

.. NOTE:: 初回アクセス後に、Webブラウザ上でJenkinsをお気に入り登録することを推奨します。

HTTPで接続するには、Webブラウザを開き、次のURLを入力します。

``http://10.1.1.8:10000``

以下のような画面が表示されます。

|image89|

.. |image78| image:: /_static/image078.png
   :align: middle
   :scale: 50%
.. |image89| image:: /_static/class2/image089.png
   :align: middle
   :scale: 50%
