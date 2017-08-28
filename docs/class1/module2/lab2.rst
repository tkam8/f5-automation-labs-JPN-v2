.. |labmodule| replace:: 2
.. |labnum| replace:: 2
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – BIG-IPデバイスのディスカバリ
-----------------------------------------------------

iWorkflowがBIG-IPデバイスと連携するためには、iWorkflowでデバイスディスカバリを実施する必要があります。デバイスディスカバリのプロセスは、BIG-IP上のCMIデバイス・トラスト・インフラストラクチャを利用します。現在のところ、BIG-IPデバイスがiWorkflowまたはBIG-IQ CMのどちらかでのみdiscoverできるという制限があります。このラボでは、ラボ環境のiWorkflowからBIG-IPデバイスディスカバリを行います。

Task 1 – BIG-IPデバイスディスカバリを実施
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクを完了するには、次の手順を実行します。

#. Postman ``Collections`` 内の ``Lab 2.2: Discover & License BIG-IP Devices`` フォルダを展開します。

#. Google ChromeでiWorkflowデバイス（https://10.1.1.6）にアクセスし、デフォルトの資格情報（admin/admin）でログインします。この画面で、Postmanで実行されるアクションを監視できます。 まず、``Devices`` を表示しておきます。

#.  ``Collections`` 内の ``Step 1：Discover BIGIP-A Device`` をクリックします。これにより、``/mgmt/shared/resolver/device-groups/cm-cloud-managed-devices/devices`` Organizing Collectionに対してデバイスディスカバリのプロセスを実行するためのPOSTが実行されます。JSONボディを調べ、ディスカバリプロセスに必要なデータを確認してください。

   |image51|

#. ``Send`` ボタンをクリックします。レスポンスを確認し、No.2で準備しておいたiWorkflow GUIを監視します。

   |image52|

#. ``BIG-IP-A`` の ``uuid`` 属性をコピーし、``iwf_bigip_a_uuid`` Postmanの環境変数に値を設定します。

   |image53|
   |image54|

#.  ``Collections`` 内の ``Step 2: Discover BIGIP-B Device`` をクリックします。

#.  ``Collections`` 内の ``Step 3: Get Discovered Devices`` をクリックします。DevicesのOrganizing CollectionをGETし、両方のBIG-IPデバイスの ``state`` が ``Active`` となっていることを確認します。

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
