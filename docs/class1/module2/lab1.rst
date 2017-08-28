.. |labmodule| replace:: 2
.. |labnum| replace:: 1
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – iWorkflowの認証
------------------------------------------------------

iWorkflowは、BIG-IP（HTTP BASIC、トークンベースの認証）と同じ認証メカニズムをサポートしています。 このラボでは、iWorkflowのトークンベースの認証を簡単にご紹介します。


Task 1 – トークンベースの認証
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクでは、ローカル認証データベースを使用してトークンベースの認証(以下、TBA)を実施しますが、外部認証プロバイダを利用した認証も利用することができます。
外部認証プロバイダの詳細については、https://devcentral.f5.com の ``iControl REST API User Guide`` の “\ **About external authentication providers with iControl REST**\ ” をご参照ください。

このタスクを完了するには、次の手順を実行します。

#. Lab 2.1のPostman Collectionの ``Step 1: Get Authentication Token`` をクリックします。

#. ``/mgmt/shared/authn/login``　エンドポイントにPOST要求を送信していることに注目してください。

   |image41|

#. ``Body`` タブをクリックし、iWorkflowに送信する資格情報を含むJSONボディを確認します。

   |image42|

#. JSONボディに必要な資格情報 ``（admin/admin）`` を追加します。次に、 ``Send`` ボタンをクリックします。

#. レスポンスステータスコードを確認します。認証が成功し、トークンが生成された場合、``200 OK`` ステータスコードが返されます。 ステータスコードが ``401`` の場合、資格情報が正しく設定されているか確認してください。

   **成功の場合:**

   - |image43|

   **失敗の場合:**

   - |image44|

#. ``200 OK`` のステータスコードを受け取ったら、レスポンスボディを確認します。様々な属性は、特定のトークンに割り当てられたパラメータを示します。次の手順で利用するために、``token`` 属性をクリップボードにコピーします（Ctrl + c）。

   |image45|

#. Lab 2.1のPostman  ``Collections`` の ``Step 2: Verify Authentication Works`` をクリックします。``Headers`` タブをクリックし、前の手順でコピーしたトークン値を ``X-F5-Auth-Token`` ヘッダのVALUEとして貼り付けます。TBAを使用する場合、このヘッダーはすべての要求で送信する必要があります。

   |image46|

#. ``Send`` ボタンをクリックします。リクエストが成功すると、``200 OK`` ステータスと ``ltm`` Organizing Collectionのリストが表示されます。

#. 次に、残りのラボでこの認証トークンを使用するようにPostman環境を更新します。Postman画面の右上にある ``Environment`` メニューをクリックし、``Manage Environments`` をクリックします。

   |image47|

#. ``INTRO – Automation & Orchestration Lab`` をクリックします。

   |image48|

#. 認証トークンに ``iwf\_auth\_token`` を貼り付けて（Ctrl-v）、値を更新します。

   |image49|

#. ``Update`` ボタンをクリックし、``Manage Environments`` ウィンドウを閉じます。 これで、後続のリクエストにトークンが自動的に組み込まれるようになります。

#. Lab 1.2のPostman collection ``Step 3: Set Authentication Token Timeout`` をクリックします。このリクエストにより、PATCH要求が送信され、トークンリソース（URIを確認）のタイムアウト属性が更新されます。リクエストのタイプとJSONボディを確認し、``Send`` ボタンをクリックします。応答でタイムアウトが ``36000`` に変更されていることを確認します。

   |image50|

.. |image41| image:: /_static/image041.png
   :scale: 40%
.. |image42| image:: /_static/image042.png
   :scale: 40%
.. |image43| image:: /_static/image043.png
   :width: 6.21017in
   :height: 0.79167in
.. |image44| image:: /_static/image044.png
   :width: 6.25278in
   :height: 0.79268in
.. |image45| image:: /_static/image045.png
   :width: 5.16635in
   :height: 2.88907in
.. |image46| image:: /_static/image046.png
   :scale: 40%
.. |image47| image:: /_static/image047.png
   :scale: 40%
.. |image48| image:: /_static/image048.png
   :width: 4.67051in
   :height: 1.23217in
.. |image49| image:: /_static/image049.png
   :scale: 40%
.. |image50| image:: /_static/image050.png
   :scale: 40%
