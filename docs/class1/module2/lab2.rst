.. |labmodule| replace:: 2
.. |labnum| replace:: 2
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – BIG-IPデバイスのディスカバリ
-----------------------------------------------------

iWorkflowがBIG-IPデバイスとやり取りするためには、iWorkflowでデバイスディスカバリを実施する必要があります。 デバイスディスカバリのプロセスは、BIG-IP上の既存のCMIデバイス・トラスト・インフラストラクチャを採用します。 現在のところ、BIG-IPデバイスがiWorkflowまたはBIG-IQ CMのどちらかでのみ ‘discover’ できるという制限があります。 このラボでは、ラボ環境から既存のBIG-IPデバイスディスカバリを行います。

Task 1 – BIG-IPデバイスディスカバリを実施
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクを完了するには、次の手順を実行します:

#. Postman Collection内の「Lab 2.2: Discover & License BIG-IP Devices」フォルダを展開します。

#. Google Chromeのウィンドウ/タブをiWorkflowデバイス（https://10.1.1.6）にアクセスし、デフォルトの資格情報（admin/admin）でログインします。この画面で、Postmanで実行されるアクションを監視できます。 まず、「Devices」ペインを表示しておきます。


#. Collection内の「Step 1：Discover BIGIP-A Device」をクリックします。これにより、 ``/mgmt/shared/resolver/device-groups/cm-cloud-managed-devices/devices``  ワーカーにデバイスディスカバリのプロセスを実行するためのPOSTが実行されます。JSONボディを調べ、ディスカバリプロセスに必要なデータを確認してください。

   |image51|

#. 「Send」ボタンをクリックします。応答を確認し、以前に開いたiWorkflow GUIを監視します。

   |image52|

#. BIGIP-Aの「uuid」属性をコピーし、「iwf\_bigip\_a\_uuid」Postmanの環境変数に値を設定します。

   |image53|
   |image54|

#. Collection内の「Step 2: Discover BIGIP-B Device」項目をクリックします。

#. Collection内の「Step 3: Get Discovered Devices」項目をクリックします。DevicesのCollectionをGETし、両方のBIG-IPデバイスの「state」が「ACTIVE」となっていることを確認します。

   |image55|

.. |image51| image:: /_static/image051.png
   :scale: 40%
.. |image52| image:: /_static/image052.png
   :width: 5.21233in
   :height: 2.73647in
.. |image53| image:: /_static/image053.png
   :scale: 40%
.. |image54| image:: /_static/image054.png
   :scale: 40%
.. |image55| image:: /_static/image055.png
   :scale: 40%
