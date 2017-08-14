.. |labmodule| replace:: 1
.. |labnum| replace:: 4
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – 基本ネットワーク接続
--------------------------------------------------------

このラボでは、次の項目の設定に焦点を当てます。

-  L1-3ネットワーク

   -  物理インターフェイスの設定

   -  L2接続 （VLAN（※）、VXLAN など）

   -  L3接続 （Self IPs（※）、 Routing（※）など）

以下のラボでは、（※）の項目を設定します。 

次の表に、BIG-IP-Aの接続を設定するために使用するL2-3ネットワーク情報を示します。

+-----------+-----------------+-------------------------+
| Type      | Name            | Details                 |
+===========+=================+=========================+
| VLAN      | Internal        | Interface: 1.1          |
|           |                 |                         |
|           |                 | Tag: 10                 |
+-----------+-----------------+-------------------------+
| VLAN      | External        | Interface: 1.2          |
|           |                 |                         |
|           |                 | Tag: 20                 |
+-----------+-----------------+-------------------------+
| Self IP   | Self-Internal   | Address: 10.1.10.1/24   |
|           |                 |                         |
|           |                 | VLAN: Internal          |
+-----------+-----------------+-------------------------+
| Self IP   | Self-External   | Address: 10.1.20.1/24   |
|           |                 |                         |
|           |                 | VLAN: External          |
+-----------+-----------------+-------------------------+
| Route     | Default         | Network: 0.0.0.0/0      |
|           |                 |                         |
|           |                 | GW: 10.1.20.254         |
+-----------+-----------------+-------------------------+

Task 1 – VLANの作成
~~~~~~~~~~~~~~~~~~~~~

.. NOTE:: このラボでは、Tag付きインターフェイスは使用しません。Tag付きインターフェイスを使用するには、 ``tagged`` 属性の値を ``true`` にする必要があります。

VLANオブジェクト/リソースを設定するには、次の手順を実行します。

#. Postman collection内の ``Lab 1.4 – Basic Network Connectivity`` フォルダを展開します。

#. Collection内の ``Step 1: Create a VLAN`` をクリックします。JSONボディを確認し、``Internal`` のVLANはすでに作成されていることを確認します。

#. VLANを作成するには、``Send`` ボタンをクリックします。

#. 手順1を繰り返します。ただし、ここではJSONボディを変更し、上記の表のパラメータを使用して ``External`` のVLANを作成します。

#. Collection内の ``Step 2: Get VLANs`` をクリックします。VLAN Collectionを取得するには ``Send`` ボタンをクリックしてください。応答を確認し、InternalとExternalのVLANが作成されていることを確認します。

Task 2 – Self IPの作成
~~~~~~~~~~~~~~~~~~~~~~~~

Self IPを設定するには、次の手順を実行します。

#. Collection内の ``Step 3: Create a Self IP`` をクリックします。JSONボディを確認し、``Self-Internal`` のSelf IPはすでに設定されていることを確認します。

#. External Self IPを作成するには、``Send`` ボタンをクリックします。

#. 手順1を繰り返します。ただし、今回はJSONボディを変更し、上記の表のパラメータを使用し ``Self-External`` のSelf IPを作成します。

#. Collection内の ``Step 4: Get Self IPs`` をクリックします。Self IP　Collectionを取得するには ``Send`` ボタンをクリックしてください。応答を確認し、両方のSelf IPが作成されていることを確認します。

Task 3 – Routeの作成（確認）
~~~~~~~~~~~~~~~~~~~~~~

Routeを設定するには、次の手順を実行します。

#. Collection内の ``Step 5: Create a Route`` をクリックします。JSONボディを確認し、Default Routeはすでに設定されていることを確認します。

#. Routeを作成するには、``Send`` ボタンをクリックします。

#. Collection内の ``Step 6: Get Routes`` をクリックします。Routes Collectionを取得するには ``Send`` ボタンをクリックしてください。応答を確認し、routeが作成されていることを確認します。
