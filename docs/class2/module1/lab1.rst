.. |labmodule| replace:: 1
.. |labnum| replace:: 1
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: Docker Community Edition (CE)のインストール
-------------------------------------------------------------------

f5-super-netops-containerを使用するには、まずシステムにDocker Community Editionをインストールする必要があります。

.. NOTE:: F5が提供するラボ環境を使用している場合、Docker CEはすでに ``Docker Server`` という名前のホストにインストールされています。そのホストにSSHで接続し、すべての ``docker`` コマンドを実行してください。

Task 1 – Docker CEをインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Docker CEをインストールするには、こちらのリンクをご参照ください。
https://docs.docker.com/engine/installation/

インストールが完了し、``hello-world`` テストを正常に実行したら、次のラボに進みます。

``hello-world`` コンテナの設定をテストするには、以下のコマンドを実行してください。

``docker run hello-world``

出力例：

.. code::

   $ sudo docker run --rm hello-world
   Unable to find image 'hello-world:latest' locally
   latest: Pulling from library/hello-world
   78445dd45222: Pull complete
   Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
   Status: Downloaded newer image for hello-world:latest

   Hello from Docker!
   This message shows that your installation appears to be working correctly.

   To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    3. The Docker daemon created a new container from that image which runs the
       executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
       to your terminal.

   To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

   Share images, automate workflows, and more with a free Docker ID:
    https://cloud.docker.com/

   For more examples and ideas, visit:
    https://docs.docker.com/engine/userguide/


.. NOTE:: ``--rm`` オプションを使うと、コンテナが停止するとすぐに削除されます。

このメッセージが表示された場合：*Cannot connect to the Docker daemon. Is the docker daemon running on this host?* 、ユーザー権限を確認してください。 また、Dockerコマンドを実行するときにsudoを使用してみてください。

hello-worldコンテナを削除するには、``sudo docker rmi hello-world`` コマンドを実行します。
コンテナが実行中の場合、イメージを削除することはできません。その場合、次のコマンドを実行します。（これにより、すべてのコンテナインスタンスが停止します）。``sudo docker stop $(docker ps -aq)``
