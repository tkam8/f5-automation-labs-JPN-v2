.. |labmodule| replace:: 2
.. |labnum| replace:: 2
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: Manually Execute a Workflow
---------------------------------------------------------

このラボでは、「f5-postman-Workflows」フレームワークを使用する2つのCollectionを取得し、Postman GUIクライアントを使用して簡単なワークフローを実行する方法について説明します。「f5-postman-workflows」 GitHubリポジトリは、F5プラットフォームを自動化するためにカスタマイズできる新しいCollectionで継続的に更新されています。 さらに、「f5-super-netops-container」は、これらのCollectionやその他のツールを自動的にダウンロードし、ワークフローの迅速な展開を支援します。

PostmanのCollectionは、**命令型**プロセスでさまざまなタスクを実行する方法の例を提供します。 Collectionを実行する際には、**命令型**プロセスへの**宣言型**の入力を定義しています。

Collectionにはドキュメントが含まれており、付属のドキュメントにアクセスしてワークフローを最初から最後まで組み込む方法について説明します。 次のラボでは、この基本知識を使用して、「f5-super-netops-container」コンテナイメージ（またはNewmanがインストールされているシステム）で「f5-newman-wrapper」によって実行できるJSONテンプレートとしてワークフローを作成します。

Task 1 - BIG-IP Collectionsのインポートと操作
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

前回のラボと同じ手順で2つのCollectionをPostmanにインポートします。 これらのCollectionにより、BIG-IPデバイスに対するREST API認証を実行し、BIG-IPデバイスで操作を実行することができます。

このタスクを完了するには、次の手順を実行します:

#. 「Import」 -> 「Import from Link」 をクリックし、以下のCollectionのURLをインポートします。

   - ``https://raw.githubusercontent.com/0xHiteshPatel/f5-postman-workflows/master/collections/BIG_IP/BIGIP_API_Authentication.postman_collection.json``
   - ``https://raw.githubusercontent.com/0xHiteshPatel/f5-postman-workflows/master/collections/BIG_IP/BIGIP_Operational_Workflows.postman_collection.json``

#. Postmanのサイドバーに2つのCollectionが追加されたことを確認します。

   - BIGIP_API_Authentication
   - BIGIP_Operational_Workflows

#. ``BIGIP_API_Authentication`` のCollectionを展開します。 コレクション内に ``1_Authenticate``　という名前のフォルダが表示されます。フォルダは、コレクション内のワークフローを整理するために使用されます。このコレクションの場合、タスク（認証用）は1つしかないため、1つのフォルダが表示されます。
#. ``1_Authenticate`` フォルダを展開します。 フォルダ内に3つの要求があることに注目してください。 これらの3つの要求は、同期的に（1つずつ）実行されます。
#. ``1_Authenticate``　フォルダの ``... ``　アイコンをクリックし、 ``Edit``　をクリックします。

   |image82|

#. 次のウィンドウには、フォルダ内のリクエストに関するドキュメントが表示されます。 さらに、一連の入力変数と出力変数が表示されます。 特に記されていない限り、すべての入力変数が必要とされます。 この場合、 ``bigip_token_timeout``　変数はオプションです。

フォルダには、異なるコレクション内の要求間でデータを渡すように設定された出力変数も含まれます。 この場合、出力変数には `` bigip_token``という名前が付けられ、 `` X-F5-Auth-Token`` HTTPヘッダーで送信されて認証を行う認証トークンが含まれます。

#. 「Cancel」をクリックしてウィンドウを閉じます。
#. 上記の手順を繰り返し、 ``BIGIP_Operational_Workflows``　Collectionを確認します。特に ``4A_Get_BIGIP_Version``　フォルダを確認してください。

Task 2 - 手動でフォルダーをワークフローに連結
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

このタスクでは、2つのフォルダを連鎖して手動でワークフローを実行する方法を説明します。 この例は単純ですが、ワークフローを構築するために一緒に組み立てたり、チェーンしたりすることができるビルディングブロックとしてフォルダを使用する方法を示しています。

``BIGIP_API_Authentication``　コレクションの ``1_Authenticate``　フォルダを使い、 ``BIGIP_Operational_Workflows``　コレクションの ``4A_Get_BIGIP_Version``　フォルダに認証トークンを渡します。

このタスクを完了するには、次の手順を実行します:

#. 「歯車アイコン」 -> 「Manage Environments」 -> 「Add」をクリックし、新しいPostmanの環境変数を作成します。
#. 環境変数に ``Lab 2.2``　という名前をつけ、以下のキーと値のペアを設定します。

   - **bigip_mgmt**: 10.1.1.4
   - **bigip_username**: admin
   - **bigip_password**: admin

#. 「Add」ボタンをクリックし、「Manage Environments」ウィンドウを閉じます。
#. ``Lab 2.2``　環境変数を選択してください。

   |image83|

前述の手順では、ワークフローを構成するすべてのフォルダに必要な入力変数を設定しました。フォルダ内のすべてのリクエストを手動で実行します。

#. ``BIGIP_API_Authentication`` -> ``1_Authenticate`` フォルダを展開します。
#. ``Authenticate and Obtain Token`` の項目を選択し、 「Send」をクリックします。
#. リクエストのレスポンス部分の ``Tests``　を確認してください。すべてのテストに合格すれば、問題はありません。さらに、次のようなテストが表示されます。

   ``[Populate Variable] bigip_token=....``

   |image85|

これらのテスト項目とそれに該当するアクション（この場合は変数を設定する）は、f5-postman-workflowsフレームワークによって生成されます。

#. Postman環境変数を確認し、 ``bigip_token``　変数が存在し、かつ設定されていることを確認してください。

   |image84|

#. フォルダ内の ``Verify Authentication Works``　リクエストを選択し、 「Send」をクリックします。 テストを確認し、すべてが合格であることを確認する

#. ``Set Authentication Token Timeout``　リクエストを選択し、 「Send」をクリックしてすべてのテストが合格であることを確認します。

この時点で、デバイス認証は成功し、認証トークンは ``bigip_token``　環境変数に格納されます。 次に、 ``bigip_token``　変数値を使用して、そのアクションを認証して実行する別のコレクションとフォルダでリクエストを実行します。

#. ``BIGIP_Operational_Workflows``　 -> ``4A_Get_BIGIP_Version``　フォルダを展開します。
#. ``Get Software Version`` リクエストをクリックします。
#. 「Headers」 タブをクリックします。``X-F5-Auth-Token``　ヘッダの値には、変数 ``bigip_token``　が設定されていることに注目してください。

   .. NOTE:: Postmanは `{{variable_name}}`　構文を使用して変数値の置換を行います。

   |image86|

#. リクエストを送信するには、「Send」をクリックします。 テストを確認し、すべてのテストが合格したことを確認します。
#. 環境変数を調べて、 ``bigip_version``　と ``bigip_build``　変数が設定されていることに注意してください。

上記の例は単純ですが、異なるコレクションやフォルダを連鎖してカスタムワークフローを組み込む方法を示しています。理解すべき重要な概念は次のとおりです。

- 「f5-postman-workflows」フレームワークとコレクションテストコードは、応答データのテストを実行し、要求が正常に実行されたことを確認します。
- 記述されているように、フレームワークは出力変数にも値を設定し、後続の要求として入力として使用することができます。

次に、この基本知識を使用し、Newmanと「f5-newman-wrapper」を使用してさまざまなコレクションとフォルダをワークフローに組み込む方法を説明します。

.. |image82| image:: /_static/image082.png
   :scale: 100%

.. |image83| image:: /_static/image083.png
   :scale: 100%

.. |image84| image:: /_static/image084.png
   :scale: 100%

.. |image85| image:: /_static/image085.png
   :scale: 100%

.. |image86| image:: /_static/image086.png
   :scale: 100%
