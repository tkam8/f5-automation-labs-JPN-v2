.. |labmodule| replace:: 2
.. |labnum| replace:: 4
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: 「f5-newman-wrapper」でワークフローを実行
-------------------------------------------------------------------

このラボでは、以前のラボで確認したワークフローを実行するために「f5-super-netops-container」コンテナを使用します。 「f5-super-netops-container」コンテナの利点は、すべてのツール、コレクション、およびフレームワークがプリインストールされ、すぐに使用できることです。

Task 1 - 「f5-newman-wrapper」ワークフローを実行
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. :ref:`previous lab <lab1_3_1>`　で説明されているようにSSHセッションを開きます。
#. ``cd f5-postman-workflows/local``　を実行します。
#. ``cp ../workflows/Wrapper_Demo_1.json .``　を実行します。
#. ``vim``　で ``Wrapper_Demo_1.json``　ファイルを編集し、 ``bigip_mgmt``　変数の値に ``10.1.1.4``　を入力してください。

   .. code:: json

        "globalVars": {
                "bigip_mgmt": "10.1.1.4",
                "bigip_username":"admin",
                "bigip_password":"admin"
        },

#. ``f5-newman-wrapper Wrapper_Demo_1.json``　を実行します。
#. 出力を調べて、ワークフローの実行方法を確認します。 ここでのテストは、以前にPostmanを使用したときのテストと同じであることに注目してください。

   出力例:

   .. code::


        [snops@f5-super-netops] [~/f5-postman-workflows/local] $ f5-newman-wrapper Wrapper_Demo_1.json
        [Wrapper_Demo_1-2017-03-30-04-08-12] starting run
        [Wrapper_Demo_1-2017-03-30-04-08-12] [runCollection][Authenticate to BIG-IP] running...
        newman

        BIGIP_API_Authentication

        ❏ 1_Authenticate
        ↳ Authenticate and Obtain Token
          POST https://10.1.1.4/mgmt/shared/authn/login [200 OK, 1.41KB, 108ms]
          ✓  [POST Response Code]=200
          ✓  [Populate Variable] bigip_token=WYKIVPHCNASNVEC55ZDVNH5OO2

        ↳ Verify Authentication Works
          GET https://10.1.1.4/mgmt/shared/authz/tokens/WYKIVPHCNASNVEC55ZDVNH5OO2 [200 OK, 1.23KB, 8ms]
          ✓  [GET Response Code]=200
          ✓  [Current Value] token=WYKIVPHCNASNVEC55ZDVNH5OO2
          ✓  [Check Value] token == WYKIVPHCNASNVEC55ZDVNH5OO2

        ↳ Set Authentication Token Timeout
          PATCH https://10.1.1.4/mgmt/shared/authz/tokens/WYKIVPHCNASNVEC55ZDVNH5OO2 [200 OK, 1.23KB, 14ms]
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
        │ total run duration: 297ms                     │
        ├───────────────────────────────────────────────┤
        │ total data received: 1.71KB (approx)          │
        ├───────────────────────────────────────────────┤
        │ average response time: 43ms                   │
        └───────────────────────────────────────────────┘
        [Wrapper_Demo_1-2017-03-30-04-08-12] [runCollection][Get BIG-IP Software Version] running...
        newman

        BIGIP_Operational_Workflows

        ❏ 4A_Get_BIGIP_Version
        ↳ Get Software Version
          GET https://10.1.1.4/mgmt/tm/sys/software/volume [200 OK, 1.32KB, 16ms]
          ✓  [GET Response Code]=200
          ✓  [Populate Variable] bigip_version=12.1.1
          ✓  [Populate Variable] bigip_build=1.0.196
        [Wrapper_Demo_1-2017-03-30-04-08-12] run completed

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
        │ total run duration: 58ms                      │
        ├───────────────────────────────────────────────┤
        │ total data received: 611B (approx)            │
        ├───────────────────────────────────────────────┤
        │ average response time: 16ms                   │
        └───────────────────────────────────────────────┘
#. ``cat Wrapper_Demo_1-env.json``　を実行し、実行終了時に保存された環境変数を確認します。

   出力例:

   .. code-block:: json
      :linenos:
      :emphasize-lines: 29-38

      {
        "id": "c0550892-36d4-4412-bf35-a1d9aa8d2efe",
        "values": [
          {
            "type": "any",
            "value": "10.1.1.4",
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
            "value": "WYKIVPHCNASNVEC55ZDVNH5OO2",
            "key": "bigip_token"
          },
          {
            "type": "any",
            "value": "1200",
            "key": "bigip_token_timeout"
          },
          {
            "type": "any",
            "value": "12.1.1",
            "key": "bigip_version"
          },
          {
            "type": "any",
            "value": "1.0.196",
            "key": "bigip_build"
          }
        ]
      }

``bigip_version``　と ``bigip_build``　変数は保存されています。このファイルはJSONでフォーマットされており、他のツールで直接自動化して簡単に使用することができます。
