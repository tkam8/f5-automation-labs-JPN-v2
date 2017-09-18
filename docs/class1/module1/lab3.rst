.. |labmodule| replace:: 1
.. |labnum| replace:: 3
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – デバイス設定の確認／設定
--------------------------------------------------------

ご利用の``BIG-IP A``デバイスは既にライセンスアクティベーションされていますので、ここでは、デバイスのオンボードプロセスを完了するために、以下のようなBIG-IPの基本設定を行います。

-  デバイス設定

   -  NTP/DNS設定

   -  リモート認証（※）

   -  ホスト名

   -  Admin資格情報

-  L1-3 ネットワーク

   -  物理インターフェースの設定

   -  L2接続（VLAN、 VXLAN（※） 等）

   -  L3接続（Self IPs、 Routing 等）

-  HA設定

   -  Global設定

      -  Config Sync IP

      -  Mirroring IP

      -  Failover Addresses

   -  CMI Device Trusts

   -  Device Groups

   -  Traffic Groups

   -  Floating Self IPs

本ラボでは、上記リストの（※）以外項目を設定します。 


Task 1 – デバイスのホスト名の設定とGUIセットアップウィザードの無効化
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクでは、デバイスのホスト名を変更し、GUIセットアップウィザードを無効にします。 
これらの設定を含むリソースは ``/mgmt/tm/sys/global-settings`` です。

このタスクを完了するには、次の手順を実行します。

#. Postmanの ``Collections`` 内の ``Lab 1.3 – Review/Set Device Settings`` フォルダを展開します。

#. ``Step 1: Get System Global-Settings`` 項目をクリックします。``Send`` ボタンをクリックし、Response Bodyにてデバイスの現在の設定内容を確認します。

#. ``Step 2: Set System Global-Settings`` をクリックします。 この項目は、``global-settings`` リソースに対するPATCHリクエストを使用して、その中に含まれる属性を変更します。 ``guiSetup`` と ``hostname`` 属性を更新します。
   - ``Body`` タブでJSONのボディを確認した後、``hostname`` 属性にて、ホスト名を ``bigip-a.f5.local`` に変更してください。
   - ``guiSetup`` 属性にて、GUIセットアップウィザードが無効となっていることを確認してください。
	|image25|

#. ``Send`` ボタンをクリックし、Response Bodyの内容を確認します。上記で変更された属性どおりに設定されていることを確認します。 
GETリクエストを再度送信することによりglobal-settingsが更新されていることも確認できます。


Task 2 – DNS/NTP設定の変更
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. NOTE:: このタスクでは、JSON配列(array)を使用します。 JSON配列を定義する構文は次のとおりです。

   ``myArray: [ Object0, Object1 ... ObjectX ]``

   文字列で構成される配列を定義する構文は次のとおりです。

   ``myStringArray: [ "string0", "string1" ... "stringX" ]``

前述のタスクと同様に、 ``sys`` というOrganizing Collection配下のリソースにPATCH要求を送信することで、システムDNSとNTP設定を更新できます。 このタスクの関連リソースは次のとおりです。

+------------------------+----------------+
| URL                    | Type           |
+========================+================+
| ``/mgmt/tm/sys/dns``   | DNS Settings   |
+------------------------+----------------+
| ``/mgmt/tm/sys/ntp``   | NTP Settings   |
+------------------------+----------------+

このタスクを完了するには、次の手順を実行します。

#. ``Collections`` 内の ``Step 3: Get System DNS Settings`` をクリックします。``Send`` をクリックし、現在の設定を確認します。

#. ``Collections`` 内の ``Step 4: Set System DNS Settings`` をクリックします。JSONボディを確認し、DNSサーバのIPアドレスに ``4.2.2.2`` と ``8.8.8.8`` がリストされていることを確認してください。さらに、``f5.local`` の検索ドメインを追加します。これらの属性の両方に対してJSON配列を変更します。

   |image89|

#. ``Send`` ボタンをクリックし、変更が正常に実装されたことを確認します。

#. ``Collections`` 内の ``Step 5: Get System NTP Settings`` をクリックします。 ``Send`` をクリックし、現在の設定を確認します。

#. ``Collections`` 内の ``Step 6: Set System NTP Settings`` をクリックします。JSONボディにて ``0.pool.ntp.org`` と ``1.pool.ntp.org`` のホスト名を持つNTPサーバが ``servers`` 属性に含まれていることを確認し、 ``timezone`` 属性を ``Japan`` に変更してください。

   |image90|

#. ``Send`` ボタンをクリックし、変更が正常に反映されたことを確認します。


Task 3 – デフォルトのユーザーアカウントのパスワードを更新
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このタスクでは、``root`` 及び ``admin`` アカウントのパスワードを更新します。rootアカウントを更新するプロセスは、他のシステムアカウントとは異なります。rootアカウントのパスワードを更新するには、 ``/mgmt/shared/authn/root`` でshared REST workerにPOSTリクエストを送信します。他のシステムアカウントを更新するには ``/mgmt/auth/user/<username>`` リソースに対してPATCHリクエストを送信します。

**root** ユーザーのパスワードを変更するには、以下の手順を実行します:

#. ``Collections`` 内の ``Step 7: Set root User Password`` をクリックします。

#. "shared" REST workerにPOST操作を実行していることに注目してください。JSONボディ内の ``newPassword`` 属性にて ``newdefault`` という値に更新し、``Send`` ボタンをクリックします。 


   |image26|


#. Puttyを起動し、設定したパスワードで ``BIG-IP-A`` にログインし、正常に変更されたことを確認します。

#. **上記の手順を繰り返し、パスワードを** ``default`` **に戻します。**

**admin** ユーザーのパスワードを変更するには、以下の手順を実行します:

#. ``Collections`` 内の ``Step 8: Set admin User Password`` をクリックします。

#. admin userリソースにPATCH操作を実行していることに注目してください。JSONボディにてパスワードを ``newadmin`` という値に更新し、``Send`` ボタンをクリックします。

   |image27|

#. PuTTYを使用してBIG-IP-AへのSSHセッションを開くか、もしくはChromeブラウザタブでTMUIにログインし、パスワードが変更されたことを確認できます。

#. **上記の手順を繰り返し、パスワードを** ``admin`` **に戻します。**

.. |image25| image:: /_static/image025.png
   :scale: 80%
.. |image26| image:: /_static/image026.png
   :scale: 40%
.. |image27| image:: /_static/image027.png
   :scale: 40%
.. |image89| image:: /_static/image089.png
   :scale: 80%
.. |image90| image:: /_static/image090.png
   :scale: 80%