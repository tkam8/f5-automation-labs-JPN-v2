.. |labmodule| replace:: 2
.. |labnum| replace:: 5
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – iWorkflow RESTプロキシ
--------------------------------------------------

Imperative（命令型）自動化ユースケースを実現するために、iWorkflowによって管理されるBIG-IPデバイスへのREST要求のパススルーを可能にするiWorkflow RESTプロキシ機能が実装されています。以下のRESTプロキシ機能を使用すると、自動化を簡素化できます。

-  BIG-IP用の集中型APIエンドポイント

   -  個々のBIG-IPデバイスと通信するのではなく、iWorkflowとだけ通信します。

-  認証の簡略化

   -  強力な認証は、各BIG-IPではなくiWorkflowで実装できます。

-  簡略化されたRBAC

   -  RBACは、環境内の個々のデバイスではなく、iWorkflowで実装できます。

RESTプロキシは、特定のURLに対して送信されたデータをBIG-IPデバイスに渡すことで動作します。 特定のデバイスのRESTプロキシのルートURLは次のとおりです:

``/mgmt/shared/resolver/device-groups/cm-cloud-managed-devices/devices/<device\_uuid>/rest-proxy/``

``…/rest-proxy/`` の後に含まれるURLセグメントはそのままBIG-IPデバイスに転送されます。 クエリーパラメータ（ ``?expandSubcollections=true`` など）も、リクエストタイプとリクエストボディとともに変更されずに渡されます。


Task 1 – RESTプロキシを通じてREST操作を実行
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクでは、RESTプロキシを通じてCRUD操作を実行します。 このタスクの目的は、これらのタスクを実行するために使用される基本的なメカニズムを示すことです。
そのデバイス用のiWorkflow REST Proxyルートを含めるようにURLを編集することで、このラボで完了した命令オペレーションをRESTプロキシを使用するように変更できます。
特定のデバイス用のiWorkflow REST Proxyルートを含めるように上記URLを変更するだけで、このラボで実施した命令型の操作を簡単に変更し、RESTプロキシを通過させることができます。

このタスクを完了するには、次の手順を実行します:

#. Postman ``Collections`` 内の ``Lab 2.5 – iWorkflow REST Proxy`` のフォルダを展開します。

#. ``Step 1: Create pool on BIGIP-A`` をクリックし、リクエストの種類、URL、JSONボディを確認します。
基本的には、 ``BIG-IP-A`` の ``/mgmt/tm/ltm/pool`` コレクションへのPOSTを実行しています。URLの最後の部分には、このURIパス（ ‘…./rest-proxy/’ の後の部分）が含まれます。JSONボディと他のパラメータは変更されずに渡されます。 また、BIG-IPではなく、iWorkflowトークンを使用して認証を行っていることに注目してください。

   |image65|

#. ``Send`` をクリックし、レスポンスを確認します。

#. ``Lab 2.5 – iWorkflow REST Proxy`` Collectionの残りの項目については、手順2〜5を実行します。何が起きているのかを理解するために、各レスポンスを確認します。

.. |image65| image:: /_static/image065.png
   :scale: 40%
