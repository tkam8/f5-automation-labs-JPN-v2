.. |labmodule| replace:: 2
.. |labnum| replace:: 5
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: 複雑なワークフローの構築
--------------------------------------------------------

以前のラボでは、非常に簡単なワークフローを検討して実行しました。 複雑なユースケースをサポートするために、「f5-newman-wrapper」には、より複雑なワークフローを構築するための機能が含まれています。

これらの機能を使用すると、以下のことを実現できます。

- 無限にネストされたアイテムを作成
- 変数名の名前の変更/再マップ
- 保存された環境ファイルから変数をロード
- グローバル（ワークフロー）またはローカル（項目）スコープで変数を定義

現在実装されているすべてのオプションを確認するには、以下のリンクを参照してください。
https://raw.githubusercontent.com/0xHiteshPatel/f5-postman-workflows/master/framework/f5-newman-wrapper/workflow-schema.json

Task 1 - ネストされたワークフローと変数のマッピング
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ワークフロー内のアイテム内の「children」配列を使用することで、ネストされたアイテムを作成できます。このタスクでは、以前のラボで使用したより高度なワークフローを作成します。 このワークフローでは、2台のBIG-IPデバイスに対する認証が実行され、ソフトウェアバージョンが取得されます。

次のブランチ図で示されるワークフローを実装します。

.. code::

   Start
     |
     |- Authenticate
     |  |- Authenticate to BIG-IP A
     |  |- Authenticate to BIG-IP B
     |
     |- Get BIGIP Version
     |  |- Get BIGIP Version on BIG-IP A
     |  |- Get BIGIP Version on BIG-IP B
     |
   Stop

このワークフローを実行するには、入力変数がワークフローの各アイテムにどのように渡されるかを理解する必要があります。 以前は、 ``BIGIP_API_Authentication``　コレクションの ``1_Authenticate``　フォルダに以下の変数が必要です。

- ``bigip_mgmt``
- ``bigip_username``
- ``bigip_password``

このワークフローを構築する際の問題は、2つのBIG-IPデバイスと通信しようとしているので、 ``bigip_mgmt``　の値が異なることです。 この問題に対処するために、入力変数を次のように定義することができます。

- ``bigip_a_mgmt = 10.1.1.4``
- ``bigip_b_mgmt = 10.1.1.5``
- ``bigip_username = admin``
- ``bigip_password = admin``

これは、両方のBIG-IP管理アドレスを提供するという問題を解決しますが、
それは別の問題を提起する。 ``1_Authenticate``　フォルダは、管理IPアドレスが ``bigip_mgmt``　入力変数に渡されることを要求します。 この問題を解決するために、各BIG-IPデバイスに対して ``1_Authenticate``　フォルダを実行する前に、変数名の再マッピングを使って「globalVar」を別の名前に再マップします。 これを説明するために、図に詳細を追加します。

.. code::

   Start
     |
     |- Define globalVars
     |  |- bigip_a_mgmt = 10.1.1.4
     |  |- bigip_b_mgmt = 10.1.1.5
     |  |- bigip_username = admin
     |  |- bigip_password = admin
     |
     |- Authenticate
     |  |- Authenticate to BIG-IP A
     |  |  | Pre-run: Remap bigip_a_mgmt -> bigip_mgmt
     |  |  |     Run: 1_Authenticate folder
     |  |
     |  |- Authenticate to BIG-IP B
     |  |  | Pre-run: Remap bigip_b_mgmt -> bigip_mgmt
     |  |  |     Run: 1_Authenticate folder
     |
     |- Get BIGIP Version
     |  |- Get BIGIP Version on BIG-IP A
     |  |- Get BIGIP Version on BIG-IP B
     |
   Stop

BIG-IP管理アドレスの定義と渡しに関する問題を解決しましたが、最後の問題を解決する必要があります。 ``1_Authenticate``　フォルダの　**出力変数**　は ``bigip_token``　です。デフォルトでは、「f5-newman-wrapper」はあるフォルダのすべての出力変数を保存し、自動的に次の項目に渡します。 この場合、 ``Authenticate to BIG-IP B``　項目は ``Authenticate to BIG-IP A``　項目で出力された ``bigip_token``　変数を上書きするために発生します。 この問題を解決するために、　**アイテムの実行後**　に変数を再マップすることができます。 以下のように、この問題に対処するために図を編集します。


.. code::

   Start
     |
     |- Define globalVars
     |  |- bigip_a_mgmt = 10.1.1.4
     |  |- bigip_b_mgmt = 10.1.1.5
     |  |- bigip_username = admin
     |  |- bigip_password = admin
     |
     |- Authenticate
     |  |- Authenticate to BIG-IP A
     |  |  |  Pre-run: Remap bigip_a_mgmt -> bigip_mgmt
     |  |  |      Run: 1_Authenticate folder
     |  |  | Post-run: Remap bigip_token -> bigip_a_token
     |  |
     |  |- Authenticate to BIG-IP B
     |  |  |  Pre-run: Remap bigip_b_mgmt -> bigip_mgmt
     |  |  |      Run: 1_Authenticate folder
     |  |  | Post-run: Remap bigip_token -> bigip_b_token
     |
     |- Get BIGIP Version
     |  |- Get BIGIP Version on BIG-IP A
     |  |- Get BIGIP Version on BIG-IP B
     |
   Stop

最後のステップは、正しいトークンを再マッピングを実行し、正しいトークンを ``4A_Get_BIGIP_Version``　フォルダに渡し、BIG-IPソフトウェアのバージョンを取得することです。 さらに、各デバイスの出力変数を保存できるように、実行後の再マップを行います。

.. code::

   Start
     |
     |- Define globalVars
     |  |- bigip_a_mgmt = 10.1.1.4
     |  |- bigip_b_mgmt = 10.1.1.5
     |  |- bigip_username = admin
     |  |- bigip_password = admin
     |
     |- Authenticate
     |  |- Authenticate to BIG-IP A
     |  |  |  Pre-run: Remap bigip_a_mgmt -> bigip_mgmt
     |  |  |      Run: 1_Authenticate folder
     |  |  | Post-run: Remap bigip_token -> bigip_a_token
     |  |
     |  |- Authenticate to BIG-IP B
     |  |  |  Pre-run: Remap bigip_b_mgmt -> bigip_mgmt
     |  |  |      Run: 1_Authenticate folder
     |  |  | Post-run: Remap bigip_token -> bigip_b_token
     |
     |- Get BIGIP Version
     |  |- Get BIGIP Version on BIG-IP A
     |  |  |  Pre-run: Remap bigip_a_mgmt -> bigip_mgmt
     |  |  |  Pre-run: Remap bigip_a_token -> bigip_token
     |  |  |      Run: 4A_Get_BIGIP_Version folder
     |  |  | Post-run: Remap bigip_version -> bigip_a_version
     |  |  | Post-run: Remap bigip_build -> bigip_a_build
     |  |
     |  |- Get BIGIP Version on BIG-IP B
     |  |  |  Pre-run: Remap bigip_b_mgmt -> bigip_mgmt
     |     |  Pre-run: Remap bigip_b_token -> bigip_token
     |     |      Run: 4A_Get_BIGIP_Version folder
     |     | Post-run: Remap bigip_version -> bigip_b_version
     |     | Post-run: Remap bigip_build -> bigip_b_build
     |
     |- Save globarVars to file
     |
   Stop

.. NOTE:: 複数のデバイス上で動作するように設計されているコレクションとフォルダは、変数を再マップする必要を避けるために、自動的に ``bigip_a _... ``　と ``bigip_b _... ``　構文を使います。 しかし、``BIGIP_Operational_Workflows``コレクションの場合は、 **一度に一つ** のデバイスに対してアクションを実行するように設計されているので、 ``bigip_token``　入力変数を再マッピングする必要があります。

.. NOTE:: この問題を解決するために使用できる別のオプションは、各項目のローカルスコープ内のすべての変数を定義することです。 この方法は、移植性が低下し、入力変数の定義が複雑になるため好ましくありません。

Task 2 - 複雑なワークフローJSONファイルを構築する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

グローバル設定と変数を定義:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
   :linenos:

   {
     "name":"Wrapper_Demo_2",
     "description":"Execute a chained workflow that authenticates to two BIG-IPs and retrieves their software version",
     "globalEnvVars":"../framework/f5-postman-workflows.postman_globals.json",
     "globalOptions": {
       "insecure":true,
       "reporters":["cli"]
     },
     "globalVars": {
       "bigip_a_mgmt": "10.1.1.4",
       "bigip_b_mgmt": "10.1.1.5",
       "bigip_username":"admin",
       "bigip_password":"admin"
     },
     "saveEnvVars":true,
     "outputFile":"Wrapper_Demo_2-run.json",
     "envOutputFile":"Wrapper_Demo_2-env.json"
   }

認証項目を定義：
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. NOTE:: 以下に示すように、 ``skip：true``　属性を使って「f5-newman-wrapper」にその特定の項目を実行しないように通知することができます。 項目「children」は依然として処理されます。 ``skip`` 属性は、同様の要求のためのコンテナを作成するために使用できます。

.. code-block:: json
   :linenos:
   :emphasize-lines: 5

   {
     "workflow": [
       {
         "name":"Authenticate to BIG-IPs",
         "skip":true,
         "children": [
           {
             "name":"Authenticate to BIG-IP A",
             "options": {
               "collection":"../collections/BIG_IP/BIGIP_API_Authentication.postman_collection.json",
               "remapPreRun": {
                 "bigip_a_mgmt": "bigip_mgmt"
               },
               "folder":"1_Authenticate",
               "remapPostRun": {
                 "bigip_token": "bigip_a_token"
               }
             }
           },
           {
             "name":"Authenticate to BIG-IP B",
             "options": {
               "collection":"../collections/BIG_IP/BIGIP_API_Authentication.postman_collection.json",
               "remapPreRun": {
                 "bigip_b_mgmt": "bigip_mgmt"
               },
               "folder":"1_Authenticate",
               "remapPostRun": {
                 "bigip_token": "bigip_b_token"
               }
             }
           }
         ]
       }
     ]
   }

上記のJSONは、ブランチ図の次の部分を実装しています:

.. code::

    |- Authenticate
       |- Authenticate to BIG-IP A
       |  |  Pre-run: Remap bigip_a_mgmt -> bigip_mgmt
       |  |      Run: 1_Authenticate folder
       |  | Post-run: Remap bigip_token -> bigip_a_token
       |
       |- Run: Authenticate to BIG-IP B
       |  |  Pre-run: Remap bigip_b_mgmt -> bigip_mgmt
       |  |      Run: 1_Authenticate folder
       |  | Post-run: Remap bigip_token -> bigip_b_token

具体的には、項目をまとめてグループ化するためのコンテナを作成するために、5行目の ``skip`` 属性の使用に注目してください。

「ソフトウェアバージョンを入手する」項目を定義
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
   :linenos:

   {
      "workflow": [
        {
          "name":"Get BIG-IP Software Versions",
          "skip":true,
          "children": [
            {
              "name":"Get BIG-IP A Software Version",
              "options": {
                "collection":"../collections/BIG_IP/BIGIP_Operational_Workflows.postman_collection.json",
                "remapPreRun": {
                  "bigip_a_mgmt": "bigip_mgmt",
                  "bigip_a_token": "bigip_token"
                },
                "folder":"4A_Get_BIGIP_Version",
                "remapPostRun": {
                  "bigip_version": "bigip_a_version",
                  "bigip_build": "bigip_a_build"
                }
              }
            },
            {
              "name":"Get BIG-IP B Software Version",
              "options": {
                "collection":"../collections/BIG_IP/BIGIP_Operational_Workflows.postman_collection.json",
                "remapPreRun": {
                  "bigip_b_mgmt": "bigip_mgmt",
                  "bigip_b_token": "bigip_token"
                },
                "folder":"4A_Get_BIGIP_Version",
                "remapPostRun": {
                  "bigip_version": "bigip_b_version",
                  "bigip_build": "bigip_b_build"
                }
              }
            }
          ]
        }
      ]
   }

上記のJSONは、ブランチ図の次の部分を実装しています：

.. code::

    |- Get BIGIP Version
       |- Get BIGIP Version on BIG-IP A
       |  |  Pre-run: Remap bigip_a_mgmt -> bigip_mgmt
       |  |  Pre-run: Remap bigip_a_token -> bigip_token
       |  |      Run: 4A_Get_BIGIP_Version folder
       |  | Post-run: Remap bigip_version -> bigip_a_version
       |  | Post-run: Remap bigip_build -> bigip_a_build
       |
       |- Get BIGIP Version on BIG-IP B
       |  |  Pre-run: Remap bigip_b_mgmt -> bigip_mgmt
          |  Pre-run: Remap bigip_b_token -> bigip_token
          |      Run: 4A_Get_BIGIP_Version folder
          | Post-run: Remap bigip_version -> bigip_b_version
          | Post-run: Remap bigip_build -> bigip_b_build

Workflow JSONファイル全体
~~~~~~~~~~~~~~~~~~~

.. code-block:: json
   :linenos:

    {
      "name":"Wrapper_Demo_2",
      "description":"Execute a chained workflow that authenticates to two BIG-IPs and retrieves their software version",
      "globalEnvVars":"../framework/f5-postman-workflows.postman_globals.json",
      "globalOptions": {
        "insecure":true,
        "reporters":["cli"]
      },
      "globalVars": {
        "bigip_a_mgmt": "",
        "bigip_b_mgmt": "",
        "bigip_username":"admin",
        "bigip_password":"admin"
      },
      "saveEnvVars":true,
      "outputFile":"Wrapper_Demo_2-run.json",
      "envOutputFile":"Wrapper_Demo_2-env.json",
      "workflow": [
        {
          "name":"Authenticate to BIG-IPs",
          "skip":true,
          "children": [
            {
              "name":"Authenticate to BIG-IP A",
              "options": {
                "collection":"../collections/BIG_IP/BIGIP_API_Authentication.postman_collection.json",
                "remapPreRun": {
                  "bigip_a_mgmt": "bigip_mgmt"
                },
                "folder":"1_Authenticate",
                "remapPostRun": {
                  "bigip_token": "bigip_a_token"
                }
              }
            },
            {
              "name":"Authenticate to BIG-IP B",
              "options": {
                "collection":"../collections/BIG_IP/BIGIP_API_Authentication.postman_collection.json",
                "remapPreRun": {
                  "bigip_b_mgmt": "bigip_mgmt"
                },
                "folder":"1_Authenticate",
                "remapPostRun": {
                  "bigip_token": "bigip_b_token"
                }
              }
            }
          ]
        },
        {
          "name":"Get BIG-IP Software Versions",
          "skip":true,
          "children": [
            {
              "name":"Get BIG-IP A Software Version",
              "options": {
                "collection":"../collections/BIG_IP/BIGIP_Operational_Workflows.postman_collection.json",
                "remapPreRun": {
                  "bigip_a_mgmt": "bigip_mgmt",
                  "bigip_a_token": "bigip_token"
                },
                "folder":"4A_Get_BIGIP_Version",
                "remapPostRun": {
                  "bigip_version": "bigip_a_version",
                  "bigip_build": "bigip_a_build"
                }
              }
            },
            {
              "name":"Get BIG-IP B Software Version",
              "options": {
                "collection":"../collections/BIG_IP/BIGIP_Operational_Workflows.postman_collection.json",
                "remapPreRun": {
                  "bigip_b_mgmt": "bigip_mgmt",
                  "bigip_b_token": "bigip_token"
                },
                "folder":"4A_Get_BIGIP_Version",
                "remapPostRun": {
                  "bigip_version": "bigip_b_version",
                  "bigip_build": "bigip_b_build"
                }
              }
            }
          ]
        }
      ]
    }

Task 3 - ワークフローを実行
^^^^^^^^^^^^^^^^^^^^^^^^^

#. :ref:`previous lab <lab1_3_1>`　で説明されているようにSSHセッションを開きます。
#. ``cd f5-postman-workflows/local``　を実行します。
#. ``cp ../workflows/Wrapper_Demo_2.json .``　を実行します。
#. ``Wrapper_Demo_2.json``　ファイルを編集し、BIG-IP管理アドレスを入力してください。

   .. code-block:: json
      :linenos:

      {
        "globalVars": {
                "bigip_a_mgmt": "10.1.1.4",
                "bigip_b_mgmt": "10.1.1.5",
                "bigip_username":"admin",
                "bigip_password":"admin"
        }
      }

#. ``f5-newman-wrapper Wrapper_Demo_2.json``　を実行します。
#. 出力を調べて、ワークフローの実行方法を確認します。

   出力例:



   .. code::

      [snops@f5-super-netops] [~/f5-postman-workflows/local] $ f5-newman-wrapper Wrapper_Demo_2.json
      [Wrapper_Demo_2-2017-03-30-19-22-52] starting run
      [Wrapper_Demo_2-2017-03-30-19-22-52] [runCollection][Authenticate to BIG-IP A] running...
      newman

      BIGIP_API_Authentication

      ❏ 1_Authenticate
      ↳ Authenticate and Obtain Token
        POST https://10.1.1.4/mgmt/shared/authn/login [200 OK, 1.41KB, 570ms]
        ✓  [POST Response Code]=200
        ✓  [Populate Variable] bigip_token=UE7W5CXWM5SJ6SZEV5A7KTAI5Q

      ↳ Verify Authentication Works
        GET https://10.1.1.4/mgmt/shared/authz/tokens/UE7W5CXWM5SJ6SZEV5A7KTAI5Q [200 OK, 1.23KB, 9ms]
        ✓  [GET Response Code]=200
        ✓  [Current Value] token=UE7W5CXWM5SJ6SZEV5A7KTAI5Q
        ✓  [Check Value] token == UE7W5CXWM5SJ6SZEV5A7KTAI5Q

      ↳ Set Authentication Token Timeout
        PATCH https://10.1.1.4/mgmt/shared/authz/tokens/UE7W5CXWM5SJ6SZEV5A7KTAI5Q [200 OK, 1.23KB, 13ms]
        ✓  [PATCH Response Code]=200
        ✓  [Current Value] timeout=1200
        ✓  [Check Value] timeout == 1200

      ┌─────────────────────────┬──────────┬──────────┐
      │                         │ executed │   failed │
      ├─────────────────────────┼──────────┼──────────┤
      │              iterations │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │                requests │        3 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │            test-scripts │        3 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │      prerequest-scripts │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │              assertions │        8 │        0 │
      ├─────────────────────────┴──────────┴──────────┤
      │ total run duration: 740ms                     │
      ├───────────────────────────────────────────────┤
      │ total data received: 1.71KB (approx)          │
      ├───────────────────────────────────────────────┤
      │ average response time: 197ms                  │
      └───────────────────────────────────────────────┘
      [Wrapper_Demo_2-2017-03-30-19-22-52] [runCollection][Authenticate to BIG-IP B] running...
      newman

      BIGIP_API_Authentication

      ❏ 1_Authenticate
      ↳ Authenticate and Obtain Token
        POST https://10.1.1.5/mgmt/shared/authn/login [200 OK, 1.41KB, 350ms]
        ✓  [POST Response Code]=200
        ✓  [Populate Variable] bigip_token=ONQXOQPNCVOHZELKIFSPHETL3I

      ↳ Verify Authentication Works
        GET https://10.1.1.5/mgmt/shared/authz/tokens/ONQXOQPNCVOHZELKIFSPHETL3I [200 OK, 1.23KB, 9ms]
        ✓  [GET Response Code]=200
        ✓  [Current Value] token=ONQXOQPNCVOHZELKIFSPHETL3I
        ✓  [Check Value] token == ONQXOQPNCVOHZELKIFSPHETL3I

      ↳ Set Authentication Token Timeout
        PATCH https://10.1.1.5/mgmt/shared/authz/tokens/ONQXOQPNCVOHZELKIFSPHETL3I [200 OK, 1.23KB, 12ms]
        ✓  [PATCH Response Code]=200
        ✓  [Current Value] timeout=1200
        ✓  [Check Value] timeout == 1200

      ┌─────────────────────────┬──────────┬──────────┐
      │                         │ executed │   failed │
      ├─────────────────────────┼──────────┼──────────┤
      │              iterations │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │                requests │        3 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │            test-scripts │        3 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │      prerequest-scripts │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │              assertions │        8 │        0 │
      ├─────────────────────────┴──────────┴──────────┤
      │ total run duration: 472ms                     │
      ├───────────────────────────────────────────────┤
      │ total data received: 1.71KB (approx)          │
      ├───────────────────────────────────────────────┤
      │ average response time: 123ms                  │
      └───────────────────────────────────────────────┘
      [Wrapper_Demo_2-2017-03-30-19-22-52] [runCollection][Get BIG-IP A Software Version] running...
      newman

      BIGIP_Operational_Workflows

      ❏ 4A_Get_BIGIP_Version
      ↳ Get Software Version
        GET https://10.1.1.4/mgmt/tm/sys/software/volume [200 OK, 1.32KB, 207ms]
        ✓  [GET Response Code]=200
        ✓  [Populate Variable] bigip_version=12.1.1
        ✓  [Populate Variable] bigip_build=1.0.196

      ┌─────────────────────────┬──────────┬──────────┐
      │                         │ executed │   failed │
      ├─────────────────────────┼──────────┼──────────┤
      │              iterations │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │                requests │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │            test-scripts │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │      prerequest-scripts │        0 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │              assertions │        3 │        0 │
      ├─────────────────────────┴──────────┴──────────┤
      │ total run duration: 250ms                     │
      ├───────────────────────────────────────────────┤
      │ total data received: 611B (approx)            │
      ├───────────────────────────────────────────────┤
      │ average response time: 207ms                  │
      └───────────────────────────────────────────────┘
      [Wrapper_Demo_2-2017-03-30-19-22-52] [runCollection][Get BIG-IP B Software Version] running...
      newman

      BIGIP_Operational_Workflows

      ❏ 4A_Get_BIGIP_Version
      ↳ Get Software Version
        GET https://10.1.1.5/mgmt/tm/sys/software/volume [200 OK, 1.32KB, 191ms]
        ✓  [GET Response Code]=200
        ✓  [Populate Variable] bigip_version=12.1.1
        ✓  [Populate Variable] bigip_build=1.0.196

      ┌─────────────────────────┬──────────┬──────────┐
      │                         │ executed │   failed │
      ├─────────────────────────┼──────────┼──────────┤
      │              iterations │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │                requests │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │            test-scripts │        1 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │      prerequest-scripts │        0 │        0 │
      ├─────────────────────────┼──────────┼──────────┤
      │              assertions │        3 │        0 │
      ├─────────────────────────┴──────────┴──────────┤
      │ total run duration: 230ms                     │
      ├───────────────────────────────────────────────┤
      │ total data received: 611B (approx)            │
      ├───────────────────────────────────────────────┤
      │ average response time: 191ms                  │
      └───────────────────────────────────────────────┘
      [Wrapper_Demo_2-2017-03-30-19-22-52] run completed in 3s, 316.921 ms

#. ``cat Wrapper_Demo_2-env.json``　を実行し、実行終了時に保存された環境変数を確認します。 BIG-IPソフトウェアのバージョンが以下に表示されています。

   Example output:

   .. code-block:: json
      :linenos:
      :emphasize-lines: 44-53,59-68

      {
        "id": "d459e491-4936-4be7-a910-567f711a636a",
        "values": [
          {
            "type": "any",
            "value": "10.1.1.4",
            "key": "bigip_a_mgmt"
          },
          {
            "type": "any",
            "value": "10.1.1.5",
            "key": "bigip_b_mgmt"
          },
          {
            "type": "any",
            "value": "10.1.1.5",
            "key": "bigip_mgmt"
          },
          {
            "type": "any",
            "value": "admin",
            "key": "bigip_username"
          },
          {
            "type": "any",
            "value": "admin",
            "key": "bigip_password"
          },
          {
            "type": "any",
            "value": "UE7W5CXWM5SJ6SZEV5A7KTAI5Q",
            "key": "bigip_a_token"
          },
          {
            "type": "any",
            "value": "ONQXOQPNCVOHZELKIFSPHETL3I",
            "key": "bigip_b_token"
          },
          {
            "type": "any",
            "value": "ONQXOQPNCVOHZELKIFSPHETL3I",
            "key": "bigip_token"
          },
          {
            "type": "any",
            "value": "12.1.1",
            "key": "bigip_a_version"
          },
          {
            "type": "any",
            "value": "1.0.196",
            "key": "bigip_a_build"
          },
          {
            "type": "any",
            "value": "1200",
            "key": "bigip_token_timeout"
          },
          {
            "type": "any",
            "value": "12.1.1",
            "key": "bigip_b_version"
          },
          {
            "type": "any",
            "value": "1.0.196",
            "key": "bigip_b_build"
          }
        ]
      }
