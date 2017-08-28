.. |labmodule| replace:: 1
.. |labnum| replace:: 1
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum| – iControl REST API
-------------------------------------------------------------

Task 1 – TMOS Web InterfaceのAPIを通して、iControl REST APIを学習しよう
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

このラボでは、TMOSに組み込まれたインターフェイスを使用してAPIの動作確認を行います。
このユーティリティは、TMOSオブジェクトがREST APIにどのようにマップされるかを理解するのに役立ちます。
これらのAPIインターフェイスはCreate、Read、Update、Delete（CRUD）機能を実装していますが、オブジェクト作成はTMUIまたはTMSHで行って、APIインターフェイスを「読み取り」ツールとして使用することが多いようです。　

#. Google Chromeを開き、３つのブックマーク（``BIG-IP A GUI``、``BIG-IP B GUI``、``iWorkflow GUI``）をクリックします。表示される証明書の警告をスキップし、各ブックマークのログイン画面が表示されることを確認します。

   |image1|

#. ブックマーク上の ``BIG-IP A REST TOC`` (https://10.1.1.4/mgmt/toc) をクリックしてください。URLの ``/mgmt/toc`` パスは、TMOSバージョン11.6以降で使用できます。

#. デフォルトのID/Password( ``admin/admin`` )を使って、ログインします。

#. ログインすると、利用可能なRESTリソースの一覧が表示されます。 ページの上部には検索ボックス |image2| があり、項目検索のために使用できます。 検索ボックスに **net** と入力し、表示結果のiControl REST Resourcesの配下にある ``net`` リンクをクリックします。

   |image3|

#. ``/mgmt/tm/net/route-domain`` という **Collection** を見つけてクリックしてください。

#. ルートドメインコレクションの一部である **Resources** のリストが表示されます。ご覧のとおり、 ``0`` のデフォルトルートドメインがリストされます。 |image4| ボタンをクリックすると、新しいオブジェクトを作成することもできます。また、 |image5| ボタンを使用してリソースを削除したり、 |image6| ボタンを使用してリソースを編集したりすることもできます。

#. ``0`` リソースをクリックすると、デバイス上のroute-domain 0の属性が表示されます。

   |image7|

   リソースへのフルパスをメモしておきます。 以下は、パスがどのように分割されているかを示しています。

   .. code::

       / mgmt / tm / net / route-domain / ~Common~0
       | Root | OC | OC  |  Collection  | Resource
       *OC=Organizing Collection

.. |image1| image:: /_static/image001.png
   :width: 6.60194in
   :height: 0.88663in
.. |image2| image:: /_static/image002.png
   :width: 0.90641in
   :height: 0.17669in
.. |image3| image:: /_static/image003.png
   :width: 2.18322in
   :height: 1.47308in
.. |image4| image:: /_static/image004.png
   :width: 0.15128in
   :height: 0.19101in
.. |image5| image:: /_static/image005.png
   :width: 0.13846in
   :height: 0.15462in
.. |image6| image:: /_static/image006.png
   :width: 0.21928in
   :height: 0.20782in
.. |image7| image:: /_static/image007.png
   :width: 6.32968in
   :height: 2.38889in
