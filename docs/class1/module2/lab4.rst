.. |labmodule| replace:: 2
.. |labnum| replace:: 4
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – L4-L7サービスのデプロイ
--------------------------------------------------

iApp Automationに基づくL4-L7サービスのデプロイを行うにあたり、iWorkflowはL4-L7サービステンプレートによるテナントサービスカタログを作成します。このデプロイモデルは、F5のL4-L7サービスのDeclarative Automationを可能にしますが、元になるiAppテンプレートはDeclarativeなプレゼンテーションレイヤ（UI）で設計する必要があります。この機能を確認するために、App Services iAppに基づいた簡単なサービスカタログテンプレートを作成し、そのテンプレートでBIG-IPテナントからアプリケーションをデプロイします。

Task 1 - iWorkflowにApp Services iAppをインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

iWorkflowは、iAppテンプレートのSource of Truth（信頼できる唯一の情報源）として機能します。
その結果、BIG-IPの展開を自動化するために使用されるiAppテンプレートは、iWorkflowにあらかじめインストールする必要があります。 テンプレートがインストールされると、iWorkflowは自動的にテンプレートをBIG-IPにインストールします。

.. NOTE:: BIG-IPデバイス上のiAppテンプレートのインストールは、 **初回** デプロイ時に行われます。

App Services iAppテンプレートデプロイを支援するために、Postman ``Collections`` が作成されています。``Collections`` をPostmanにインポート後に、iWorkflowにテンプレートをインストールします。

このタスクを完了するには、次の手順を実行します。

#. ``Import`` -> ``Import from Link`` から、以下のCollection URLをインポートします。

   .. parsed-literal::

      :raw_github_url:`/postman_collections/AppSvcs_iApp_Workflows.postman_collection.json`

#. ``AppSvcs_iApp_Workflows`` Collectionを展開し、``2_Install_on_iWorkflow`` フォルダを開き、``Install AppSvcs Template on iWorkflow`` をクリックします。　　　 

#. このリクエストのボディを確認することはできますが、iAppのコードの改行や空白が取り除かれているため、読みにくい場合があります。このコレクションは、インストールを簡単にするために既に設定されている基本変数( ``iwf_mgmt`` や ``iwf_auth_token`` )を使用します。

#. ``Send`` ボタンをクリックし、iAppをインストールします。

Task 2 – f5-http-lb L4-7サービステンプレートの作成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

iWorkflow上のL4-L7サービスデプロイメントは、L4-L7サービステンプレートの作成から始まります。これらのテンプレートを使用すると、プロバイダ（管理者）はiAppプレゼンテーションレイヤ（UI）の特定のフィールドの値を指定できます。さらに、プロバイダは、テナントがサービスを展開するときに表示されるパラメータを定義する ‘\ **Tenant Editable**\ ’ 設定を使用することができます。基本的には、サービスカタログテンプレートは、サービスを展開するために必要な最小限のフィールドセットを公開するフィルタとして機能します。また、特定のパラメータのデフォルト値を設定することもできます。

このタスクでは、以前にインストールしたApp Services iAppを利用するサービスカタログテンプレートを作成します。

このタスクを完了するには、次の手順を実行します。

#. ``Collections`` 内の ``AppSvcs_iApp_Workflows`` 配下の ``3_iWorkflow_Service_Templates_Examples`` フォルダを展開します。

#. ``Collections`` 内の ``f5-http-lb Template`` をクリックします。このリクエストはあらかじめ作成されており、App Services iAppを使った新しいサービステンプレートを作成します。 ``Send`` ボタンをクリックし、テンプレートを作成します。

#. ChromeでiWorkflow（https://10.1.1.6）にアクセスし、資格情報（admin/admin）でログインします。 ``Service Templates`` ペインを展開し、``f5-http-lb`` テンプレートをダブルクリックします。デフォルトでさまざまな値が設定されていること確認してください（例えば、 ``pool__port`` 変数のポート ``80`` ）。また、一部のフィールドには ``Tenant Editable`` とマークされています。

   |image59|

Task 2 – テナントのL4-L7 サービス展開
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクでは、前のタスクで作成したサービスカタログテンプレートの展開に基づいてCRUD操作を実行します。

※CRUDとはCreate（作成）、Read（読み取り）、Update（更新）、Delete（削除）機能をまとめた表現です。

このタスクを完了するには、次の手順を実行します。

#. ChromeでiWorkflow（https://10.1.1.6）にアクセスし、Username: tenant, Password: tenantの資格情報でログインします。そして、``L4-L7 Services`` ペインを展開します。

#. F5 Automation & Orchestration Intro Postman Collectionに戻り、Lab 2.4の ``Step 1: Create TENANT Service Deployment`` をクリックし、URLとJSONボディを確認します。そして ``MyTenant`` 配下に、``Tenant Editable`` とマークされたパラメータを持つ新しいテナントのサービス展開を作成します。この項目は、サービス展開のCreate（作成）操作の例です。

   |image60|

#. ``Send`` ボタンをクリックし、サービス展開を作成後に、レスポンスを確認します。 iWorkflow GUIでは、``Services`` ペインに新しい項目が表示されていることを確認します。

   |image61|

#. 新しいChromeタブで、BIGIP-Aにアクセスします。``iApps`` -> ``Application Services`` -> ``Applications`` -> ``example-f5-http-lb`` をクリックし、BIG-IPにデプロイされた設定を確認します。

   |image62|

#. Postmanに戻り、 ``Collections`` 内の ``Step 2: Get TENANT Service　Deployment`` 項目をクリックし、``Send`` をクリックします。 この項目は、サービス展開のRead（読み取り）操作の例です。 レスポンスは、iWorkflow GUIのデプロイメントプロパティの画面に表示される設定と一致することを確認します。

#. ``Collections`` 内の ``Step 3: Modify TENANT Service Deployment`` をクリックします。 この要求は、Update（更新）操作の例です。サービスデプロイメントを示すURLに対してPUTリクエストを送信していることに注目してください。JSONボディを解析し、``pool__Members`` テーブルにIPアドレス10.1.10.12の新しいプールメンバーが追加されていることを確認します。``Send`` ボタンをクリックし、サービスを再デプロイします。

   |image63|

#. プールメンバーがBIG-IPに追加されたことを確認します。

   |image64|

#. Postmanに戻り、``Step 4: Delete TENANT Service Deployment`` 項目をクリックします。 この項目は、デプロイ先サービスのURLに対してDELETE要求を送信します。``Send`` をクリックし、iWorkflow及びBIG-IP GUIでデプロイメントが削除されていることを確認します。

.. |image59| image:: /_static/image059.png
   :scale: 40%
.. |image60| image:: /_static/image060.png
   :scale: 40%
.. |image61| image:: /_static/image061.png
   :scale: 40%
.. |image62| image:: /_static/image062.png
   :scale: 40%
.. |image63| image:: /_static/image063.png
   :scale: 40%
.. |image64| image:: /_static/image064.png
   :scale: 40%

