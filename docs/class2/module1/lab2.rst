.. |labmodule| replace:: 1
.. |labnum| replace:: 2
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: 「f5-super-netops-container」イメージのダウンロードと起動
--------------------------------------------------------------------------------

このラボでは、「f5-super-netops-container」イメージをダウンロードして起動するための ``docker`` cliツールを使用します。

Task 1 – コンテナイメージをダウンロード
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクを完了するには、次の手順を実行します:

#. コマンドプロンプトを開きます。

.. NOTE:: F5提供のラボ環境を使用している場合は、SSHで「Docker Server」ホストに接続し、次のコマンドを実行してください。

#. ``docker pull f5devcentral/f5-super-netops-container:develop-jenkins``　を実行します。

   出力例:

   .. code::

      $ docker pull f5devcentral/f5-super-netops-container:develop-jenkins
      develop-jenkins: Pulling from f5devcentral/f5-super-netops-container
      019300c8a437: Pull complete
      2d6b38b56ae7: Pull complete
      5fab9174d5b4: Pull complete
      fc0251c85d81: Pull complete
      d5c1476cba25: Pull complete
      3f563aeb530f: Pull complete
      56717b902584: Pull complete
      3a973f5ee17d: Pull complete
      68d52f474d41: Pull complete
      604d6366bf0b: Pull complete
      b3b4184aef22: Pull complete
      2cebe1f5955c: Pull complete
      2b7bce9d0d9e: Pull complete
      259f696f7766: Pull complete
      6d5f2e57c5b3: Pull complete
      985706ad6d05: Pull complete
      a29f68892227: Pull complete
      7420ee096abd: Pull complete
      0907797bbe90: Pull complete
      5b8f2518bf01: Pull complete
      2940be145e35: Pull complete
      f2cb35cbf665: Pull complete
      5cdfa1779954: Pull complete
      61c1367b68d8: Pull complete
      5bcd8c5223bb: Pull complete
      b0defdb83b82: Pull complete
      Digest: sha256:27563f98bf58c9d26eb5989acaf540a9ad7fb1806e4a4c373ad28769ebe63ef4
      Status: Downloaded newer image for f5devcentral/f5-super-netops-container:develop-jenkins

#.  ``docker images``　を実行します。

   出力例:

   .. code::

      $ docker images
      REPOSITORY                               TAG                 IMAGE ID            CREATED             SIZE
      f5devcentral/f5-super-netops-container   develop-jenkins     b71fc40407e4        2 weeks ago         490MB

Task 2 – Start the container image
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

コンテナを起動するには、次のコマンドを実行します。

``docker run -p 8080:80 -p 2222:22 -p 10000:8080 --rm -it -e SNOPS_GH_BRANCH=develop f5devcentral/f5-super-netops-container:develop-jenkins``

``-p``　オプションで、外部からアクセスされるポート番号:コンテナ側のポート番号と指定し、ポートフォワードをさせます。
例えば、 ``-p 8080：80`` オプションは、ホスト側のポート ``8080`` をコンテナのポート ``80`` にリダイレクトします。

``-it`` オプションをつけることでコンテナ内で操作できます。

``-e`` [変数名]=[値]オプションで、GitHubのブランチ（Master/Develop等）を指定します。 このラボでは、 ``develop``　を指定します。

``f5devcentral/f5-super-netops-container:develop-jenkins`` オプションは、前のタスクでダウンロードしたイメージの名前です。

このタスクを完了するには、次の手順を実行します:

#. ``docker run -p 8080:80 -p 2222:22 -p 10000:8080 --rm -it -e SNOPS_GH_BRANCH=develop f5devcentral/f5-super-netops-container:develop-jenkins``　を実行します。

   .. NOTE:: このイメージには、ツールとドキュメントの最新バージョンをダウンロードするためにインターネットに接続する必要があります。 イメージを起動する前に、ホストからのインターネットアクセスがあることを確認してください。プロキシを使用する必要がある場合は、以下のドキュメントを参照してください。
   
      https://docs.docker.com

   このコマンドで、イメージが起動され、インターネットからリソースがロードされます。この処理には、接続速度に応じて時間がかかることがあります。起動プロセスが完了すると、 `` root`` ユーザプロンプトが表示されます。標準のLinuxコマンドを使用してイメージとやり取りすることができます。次のラボでは、SSHとHTTP経由でイメージに接続します。

   起動時の出力例：

   .. code::

      container:develop-jenkins
      [s6-init] making user provided files available at /var/run/s6/etc...exited 0.
      [s6-init] ensuring user provided files have correct perms...exited 0.
      [fix-attrs.d] applying ownership & permissions fixes...
      [fix-attrs.d] done.
      [cont-init.d] executing container initialization scripts...
      [cont-init.d] done.
      [services.d] starting services
      [services.d] done.
      [environment] SNOPS_HOST_SSH=2222
      [environment] SNOPS_REPO=https://github.com/f5devcentral/f5-super-netops-container.git
      [environment] SNOPS_AUTOCLONE=1
      [environment] SNOPS_HOST_IP=172.17.0.2
      [environment] SNOPS_ISALIVE=1
      [environment] SNOPS_GIT_HOST=github.com
      [environment] SNOPS_REVEALJS_DEV=0
      [environment] SNOPS_HOST_HTTP=8080
      [environment] SNOPS_IMAGE=jenkins
      [environment] SNOPS_GH_BRANCH=develop
      Reticulating splines...
      Becoming self-aware...
      [cloneGitRepos] Retrieving repository list from https://github.com/f5devcentral/f5-super-netops-container.git#develop
      [updateRepos] Processing /tmp/snops-repo/images/jenkins/fs/etc/snopsrepo.d/jenkins.json
      [updateRepos]  Processing /tmp/snops-repo/images/base/fs/etc/snopsrepo.d/base.json
      [updateRepos] Processing /tmp/user_repos.json
      [cloneGitRepos] Loading repositories from /home/snops/repos.json
      [cloneGitRepos] Found 7 repositories to clone...
      [cloneGitRepos][1/7] Cloning f5-sphinx-theme#master from https://github.com/f5devcentral/f5-sphinx-theme.git
      [cloneGitRepos][1/7]  Installing f5-sphinx-theme#master
      [cloneGitRepos][2/7] Cloning f5-super-netops-container#develop from https://github.com/f5devcentral/f5-super-netops-container.git
      [cloneGitRepos][2/7]  Installing f5-super-netops-container#develop
      [cloneGitRepos][3/7] Cloning f5-application-services-integration-iApp#develop from https://github.com/F5Networks/f5-application-services-integration-iApp.git
      [cloneGitRepos][3/7]  Installing f5-application-services-integration-iApp#develop
      [cloneGitRepos][4/7] Cloning f5-postman-workflows#develop from https://github.com/0xHiteshPatel/f5-postman-workflows.git
      [cloneGitRepos][4/7]  Installing f5-postman-workflows#develop
      [cloneGitRepos][5/7] Cloning f5-automation-labs#master from https://github.com/f5devcentral/f5-automation-labs.git
      [cloneGitRepos][5/7]  Installing f5-automation-labs#master
      [cloneGitRepos][6/7] Cloning ultimate-vimrc#master from https://github.com/amix/vimrc.git
      [cloneGitRepos][6/7]  Installing ultimate-vimrc#master
      [cloneGitRepos][7/7] Cloning reveal-js#master from https://github.com/hakimel/reveal.js.git
      [cloneGitRepos][7/7]  Installing reveal-js#master
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

      (you can now detach by using Ctrl+P+Q)

      [root@f5-super-netops] [/] #

Task 3 - コンテナの取り外し/再取り付け（Detach/Attach）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

フォアグラウンドプロセス（この場合はシェル）が終了すると、コンテナが終了することを理解することが重要です。 たとえば、実行中のコンテナに `` exit`` コマンドを入力した場合、シャットダウンプロセスが開始されます。 これを避けるには、起動が完了したらコンテナから取り外す必要があります。 その後、SSHでコンテナに接続し、コンテナ内の操作を実行することができます。 これについては次のラボで説明します。

コンテナを取り外し
^^^^^^^^^^^^^^^^^^^^

#. 起動中のTTY（仮想端末）で ``Ctrl+p+q`` を押します。

   出力例:

   .. code::

      [root@f5-super-netops] [/] #
      [root@f5-super-netops] [/] #
      [root@f5-super-netops] [/] # <enter Ctrl+p+q>
      hostname:~ user$

#. ``docker ps``　を入力し、コンテナがまだ起動していることを確認します。

   出力例:

   .. code::

      hostname:~ user$ docker ps
      $ docker ps
      CONTAINER ID        IMAGE                                                    COMMAND                  CREATED             STATUS              PORTS                                                                                      NAMES
      4cf75944bfbc        f5devcentral/f5-super-netops-container:develop-jenkins   "/init /snopsboot/..."   2 minutes ago       Up 2 minutes        8000/tcp, 50000/tcp, 0.0.0.0:2222->22/tcp, 0.0.0.0:8080->80/tcp, 0.0.0.0:10000->8080/tcp   loving_montalcini

コンテナを再取り付け
^^^^^^^^^^^^^^^^^^^^^^^

#. ``docker ps``　を実行します。

   出力例:

   .. code::

       hostname:~ user$ docker ps
       $ docker ps
       CONTAINER ID        IMAGE                                                    COMMAND                  CREATED             STATUS              PORTS                                                                                      NAMES
       4cf75944bfbc        f5devcentral/f5-super-netops-container:develop-jenkins   "/init /snopsboot/..."   2 minutes ago       Up 2 minutes        8000/tcp, 50000/tcp, 0.0.0.0:2222->22/tcp, 0.0.0.0:8080->80/tcp, 0.0.0.0:10000->8080/tcp   loving_montalcini
      |------------|
        ^- YOUR CONTAINER ID

#. ``CONTAINER ID``　カラムに、「f5devcentral/f5-super-netops-container:develop-jenkins」イメージに該当する値をコピーします。
#. ``docker attach <container_id>`` を実行します。
#. コマンドプロンプトを表示するには ``<Enter>``　を押す必要があります。
#. ``<Ctrl+p+q>``　入力し、もう一度コンテナを取り外します。
