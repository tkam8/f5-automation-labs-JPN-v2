.. |labmodule| replace:: 2
.. |labnum| replace:: 3
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: ``f5-newman-wrapper`` ツールの紹介
------------------------------------------------------------

これまでのラボでご紹介したように、Postman GUIを使用してコレクションやフォルダを手動で実行し、BIG-IPデバイスの操作をすることができます。手動実行はテスト段階では重要ですが、本番稼動後は自動プロセスとして実行できるようにする必要があります。

この目標を達成するために、 ``f5-newman-wrapper`` ツールを使用します。 このツールを使用すると、ユーザーはワークフローをJSON形式のファイルで指定できます。 このワークフローには、入力変数、ツールを実行するコレクションとフォルダ、出力結果を表示するための様々な出力オプションが含まれます。

``f5-newman-wrapper`` を利用したワークフローの中核要素は、JSON形式の入力ファイルです。 このラボでは、そのファイルフォーマットを紹介します。

Task 1 - ワークフロー JSONフォーマットのご紹介
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ワークフローのファイルフォーマットを紹介するにあたり、これまでのラボで手動で実行した簡単なワークフローを再作成する例を使用します。 まず、セクション毎でファイルを解析し、その後ファイル全体を確認します。


ネームとディスクリプションを定義
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
   :linenos:

   {
        "name":"Wrapper_Demo_1",
        "description":"Execute a chained workflow that authenticates to a BIG-IP and retrieves it's software version"
   }


実行時のグローバル設定を定義
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このセクションでは、 ``f5-newman-wrapper`` がこのワークフローをどのように実行するかを定義します。 属性は以下の表で説明されています。

.. code-block:: json
   :linenos:

   {
        "globalEnvVars":"../framework/f5-postman-workflows.postman_globals.json",
        "globalOptions": {
            "insecure":true,
            "reporters":["cli"]
        },
        "saveEnvVars":true,
        "outputFile":"Wrapper_Demo_1-run.json",
        "envOutputFile":"Wrapper_Demo_1-env.json"
    }

.. list-table::
    :header-rows: 1
    :widths: 20 80
    :stub-columns: 1

    * - **Attribute**
      - **Description**
    * - ``globalEnvVars``
      - Newmanによって使われるグローバル環境変数を含むファイルです。このファイルは、f5-postman-workflowsスクリプトによって作成されます。このラボは、これまでのラボで使ってきたのと同じグローバル変数を使います。
    * - ``globalOptions``
      - Newmanのグローバルオプションを指定できます。設定可能なオプションは、以下のURLをご覧下さい。
        https://github.com/postmanlabs/newman#api-reference

        .. NOTE:: ``reporters`` 配列から ``cli`` オプションを削除すると、CLIが利用できなくなるので注意してください。

    * - ``saveEnvVars``
      - ワークフロー実行時の最後に、環境変数をファイルに保存する
    * - ``outputFile``
      - ワークフロー実行詳細を保存するファイル名
    * - ``envOutputFile``
      - ワークフロー実行時の最後に、環境変数を保存するファイル名


入力変数を定義
~~~~~~~~~~~~~~~~~~~~~~

このセクションでは、ワークフローの入力変数を指定します。``globalVars`` は、ここで定義された変数が、定義されたワークフロー（ワークフローの視点からのグローバルスコープ）の各要求に対して存在することを意味します。変数は、ワークフローの各項目（項目の観点から見たローカルスコープ）内で定義することもできます。グローバル変数とローカル変数が同じ名前の場合は、ローカルスコープ変数が優先されます。

.. code-block:: json
   :linenos:

    {
        "globalVars": {
            "bigip_mgmt": "10.1.1.4",
            "bigip_username":"admin",
            "bigip_password":"admin"
        }
    }



ワークフローコレクションと実行順序の定義
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このセクションでは、構成されているワークフロー、コレクションおよびフォルダを定義します。``workflow`` 属性は、実行する各コレクションと、フォルダを定義するオブジェクトを含む順序付けられた配列です。

.. code-block:: json
   :linenos:

    {
       "workflow": [
           {
               "name":"Authenticate to BIG-IP",
               "options": {
                   "collection":".. /collections/BIG_IP/BIGIP_API_Authentication.postman_collection.json",
                   "folder":"1_Authenticate"
               }
           },
           {
               "name":"Get BIG-IP Software Version",
               "options": {
                   "collection":"../collections/BIG_IP/BIGIP_Operational_Workflows.postman_collection.json",
                   "folder":"4A_Get_BIGIP_Version"
               }
           }
       ]
   }

認証を実行するワークフロー内の項目を見てみましょう。

.. code-block:: json
   :linenos:

                   {
                           "name":"Authenticate to BIG-IP",
                           "options": {
                                   "collection":".. /collections/BIG_IP/BIGIP_API_Authentication.postman_collection.json",
                                   "folder":"1_Authenticate"
                           }
                   }

``name`` 属性は、このアイテムの名前を指定します。``options`` オブジェクトは、この特定の項目を実行するために使用されるパラメータを指定します。上記の例では、``collection`` 属性は ``BIGIP_API_Authentication`` コレクションを含むファイルを参照します。``folder`` 属性は、コレクション内で実行するフォルダの名前を指定します。

デフォルトでは、コレクションまたはフォルダのすべての出力変数は、ワークフローの次の項目に渡されます。これにより、コレクションを連鎖させてワークフローを構築することができます。

Workflow JSONファイル全体
~~~~~~~~~~~~~~~~~~~

.. code-block:: json
   :linenos:

   {
           "name":"Wrapper_Demo_1",
           "description":"Execute a chained workflow that authenticates to a BIG-IP    and retrieves it's software version",
           "globalEnvVars":"../framework/f5-postman-workflows.postman_globals.json",
           "globalOptions": {
                   "insecure":true,
                   "reporters":["cli"]
           },
           "globalVars": {
                   "bigip_mgmt": "10.1.1.4",
                   "bigip_username":"admin",
                   "bigip_password":"admin"
           },
           "saveEnvVars":true,
           "outputFile":"Wrapper_Demo_1-run.json",
           "envOutputFile":"Wrapper_Demo_1-env.json",
           "workflow": [
                   {
                           "name":"Authenticate to BIG-IP",
                           "options": {
                                   "collection":"..   /collections/BIG_IP/BIGIP_API_Authentication.   postman_collection.json",
                                   "folder":"1_Authenticate"
                           }
                   },
                   {
                           "name":"Get BIG-IP Software Version",
                           "skip":false,
                           "options": {
                                   "collection":"..   /collections/BIG_IP/BIGIP_Operational_Workflows.   postman_collection.json",
                                   "folder":"4A_Get_BIGIP_Version"
                           }
                   }
           ]
   }
