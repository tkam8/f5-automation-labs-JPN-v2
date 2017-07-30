.. |labmodule| replace:: 2
.. |labnum| replace:: 3
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – テナントとBIG-IPコネクタの作成
--------------------------------------------------------------

iWorkflowは、様々な環境へL4-7サービスの抽象化された展開を可能にするためにテナント/プロバイダインタフェースを実装しています。
iWorkflowコネクタは、オートメーションツールチェーンのL1-3ネットワーク及びデバイスオンボーディングの自動化コンポーネントとして機能します。
また、iWorkflowはさまざまなベンダー統合のためのコネクタをサポートしています（F5 vCMP、F5 BIG-IP、Cisco APIC、VMware NSXなど）。
このラボでは、BIG-IPデバイス用の「BIG-IP Connector」を作成します。 このコネクタを使用すると、iWorkflowサービスカタログから完全に自動化された展開を実行できます。

Task 1 – テナントとテナントのユーザーを作成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクでは、BIG-IPデバイスにリンクされたローカルコネクタを作成します。
ローカルコネクタはDSC(冗長化機能)に対応しており、BIG-IPデバイスがクラスタ化されていることを自動的に検出し、それに応じて構成されます。

このタスクを完了するには、次の手順を実行します:

#. Postman Collection内の「Lab 2.3 – Create Tenant & Local Connector」のフォルダを展開します。

#. Collection内の「Step 1: Create iWorkflow Tenant」をクリックし「Send」をクリックします。この要求により、``MyTenant``というテナントが作成されます。

#. Collection内の「Step 2: Create Tenant User」をクリックし「Send」をクリックします。
   この要求により、テナントユーザ名 **tenant** が作成されます。

#. Collection内の「Step 3: Assign User to Tenant Admin Role」をクリックし、「Send」をクリックします。このリクエストは `` MyTenant`` テナントの `` tenant`` ユーザに管理者ロールを割り当てます。
`` tenant``Adminロールを割り当てます。


Task 2 - ローカルコネクタを作成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Collection内の「Step 4: Create a Local Connector」をクリックします。ローカルコネクタのCollectionへのPOSTを実行して、新しいコネクタを作成します。JSONボディを確認すると、BIG-IP-AデバイスのURLへの参照（以前に入力したUUID環境変数）が提供されていることがわかります。

   |image56|

#. 「Send」ボタンをクリックしてコネクタを作成します。

#. Collection内の「Step 5: Get Local Connectors」をクリックし、「Send」をクリックします。JSONボディで、コネクターの構成を確認します。次に、「device-group」への参照に注目してください。これは、コネクタがBIG-IPデバイスのHA状態を確認する方法です。コネクタの「connectorId」を探し、「connectorId」を「iwf\_connector\_uuid」変数の値として含めるようにPostman環境を更新します。

   |image57|
   |image58|

#. Collection内の「Step 6: Assign Connector to Tenant」をクリックします。この要求は、このコネクタを「MyTenant」テナントに割り当て、そのテナントからのサービス展開を可能にします。「Send」ボタンをクリックし、応答を確認します。

.. |image56| image:: /_static/image056.png
   :scale: 40%
.. |image57| image:: /_static/image057.png
   :width: 5.24968in
   :height: 2.77172in
.. |image58| image:: /_static/image058.png
   :scale: 40%
