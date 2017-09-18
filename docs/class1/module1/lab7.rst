.. |labmodule| replace:: 1
.. |labnum| replace:: 7
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – REST APIのトランザクション
---------------------------------------------------

Task 1 – トランザクションを作成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このラボでは、REST APIを使用してトランザクションを作成します。トランザクションは、個別のREST操作を単一のバッチ操作として実行する場合に使用されます。トランザクションを実行すると、すべてのコマンドが順番にキューに入れられ、失敗した場合はすべての変更がロールバックされます。

また、キューに入れられたコマンドの順序を変更する方法や、コミットする前にキューに入れられたリストから単一のコマンドを削除する方法についてもレビューします。

このタスクを完了するには、次の手順を実行します:

#. Postman ``Collections`` 内の ``Lab 1.7 – Rest API Transactions`` フォルダを展開します。

   |image35|

#. ``Step 1: Create a Transaction`` の項目をクリックし、URL、とJSONボディを確認します。新しいトランザクションを作成するために、空のJSONボディを含むPOSTを ``/mgmt/tm/transaction`` RESTワーカーに対して送信します。

   |image36|

#. リクエストを送信するには ``Send`` ボタンをクリックします。レスポンスを確認し、``transId`` 属性を見つけます。

   |image37|

#. この属性の値を ``bigip_transaction_id`` 環境変数に保存してください。さらに、トランザクションのSubmitと実行時間の両方にタイムアウトがあること、そして ``timeoutSeconds`` 値を超えると、このtransIdが削除されることにも注目してください。

   |image38|

#. Postman  ``Collections`` 内の ``Step 2: Add to Transaction: Create a HTTP Monitor`` 項目をクリックします。この要求は、non-transaction enabled requestと同じ要求タイプ（POST）、URI、およびJSONボディとなっています。違いは ``X-F5-REST-Coordination-Id`` ヘッダに ``transId`` 属性の値を加えてトランザクションに追加する点です。

   |image39|

#. ``Send`` ボタンをクリックし、応答を確認します。

#.  ``Collections`` 内のステップ3〜6で ``Send`` をクリックし、確認します。

#. ``Step 7: View the Transaction queue`` の項目をクリックし、リクエストタイプとURIを確認してから ``Send`` ボタンをクリックします。この要求によって、トランザクション内にあるコマンド（順序付けられた）の現在のリストを表示することができます。

Task 2 – トランザクションの変更
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#.  ``Collections`` 内の ``Step 8: View queued command 4 from Transaction`` 項目をクリックし、リクエストタイプとURIを確認します。キューに入れられたコマンド＃4をトランザクションリストから取得します。

   |image76|

#.  ``Collections`` 内の ``Step 9: Change Eval Order 4 ->1`` 項目をクリックし、リクエストタイプ、URI、とJSONボディを確認します。トランザクションリソースにPATCHリクエストを送信後に、``evalOrder`` 属性の値を4から1に変更し、トランザクションキューの最初の位置に移動します。

   |image77|

#.  ``Collections`` 内の ``Step 10: View the Transaction queue changes`` 項目をクリックします。トランザクション#4が位置1に移動し、他のすべてのトランザクションの ``evalOrder`` がそれに応じて移動したことを確認します。

Task 3 – トランザクションのコミット
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#.  ``Collections`` 内の ``Step 11: Commit the Transaction`` 項目をクリックし、リクエストタイプ、URI、とJSONボディを確認します。トランザクションリソースにPATCH要求を送信し、トランザクションをsubmitするための ``state`` 属性の値を変更します。

   |image40|

#. ``Send`` ボタンをクリックし、応答を確認します。

#. TMUIまたはREST要求で設定が作成されたことを確認します。

.. |image35| image:: /_static/image035.png
   :width: 4.09062in
   :height: 2.93314in
.. |image36| image:: /_static/image036.png
   :scale: 40%
.. |image37| image:: /_static/image037.png
   :width: 5.66944in
   :height: 2.55359in
.. |image38| image:: /_static/image038.png
   :scale: 90%
.. |image39| image:: /_static/image039.png
   :scale: 100%
.. |image40| image:: /_static/image040.png
   :scale: 90%
.. |image76| image:: /_static/image076.png
   :scale: 90%
.. |image77| image:: /_static/image077.png
   :scale: 90%

.. WARNING:: ``X-F5-REST-Coordination-Id`` ヘッダーを送信するとき、システムはトランザクションキューにエントリを追加すると仮定します。トランザクション・キューの変更（キューからのエントリの削除、順序の変更、トランザクションのコミットなど）を発行する場合は、このヘッダーを削除する必要があります。その特定のケースでヘッダーを削除しないと、システムはHTTPエラー400の次のようなエラーを送信します。 "message": "Transaction XXXXX operation .... is not allowed to be added to transaction."
