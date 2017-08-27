Module 2 – iWorkflow
====================

このモジュールでは、F5のiWorkflowプラットフォームを使用してアプリケーションサービスを抽象化し、それらのサービスをテナントに提供する方法について説明します。
iWorkflowはAutomation＆Orchestrationのツールチェーンに2つの主な目的を持っています：

-  簡素化されたカスタマイズ可能なデバイスオンボーディングのワークフローを提供すること

-  L4 - L7サービスデリバリのためにテナント/プロバイダインタフェースを提供すること

iWorkflowベースのツールチェーンに移行する際には、L1-3の自動化(Device Onboarding, Networking, 等)とL4-7の自動化(Deployment of Virtual Servers, Pools, 等)が分離され、
異なる機能によって提供されることを理解することが重要です。

L1-3ネットワークとデバイスオンボーディングは、サードパーティのテクノロジエコシステムに固有の ``Cloud　Connectors`` によって提供されます。
(例： vCMP, Cisco APIC, VMware NSX, BIG-IP, 等)

L4-7サービスの提供は、次のモデルによって実現できます:

-  宣言型(Declarative)モデル：BIG-IPデバイス上のF5 iAppテンプレートを利用してサービスカタログを作成すること

-  命令型(Imperative)モデル：iWorkflow RESTプロキシを利用してBIG-IPデバイスへのAPI呼び出しを駆動すること

このモジュールのラボでは、L1-7の完全自動化を実現するための高度な機能に焦点を当てます。 上記のように、iAppsはこのツールチェーンの主要コンポーネントです。
従って、f5.httpというiAppを使用してシンプルな導入例を作成します。
より高度なユースケースの場合は、 ``Declarative`` または ``Deployment-centric`` (デプロイメント中心)のiAppテンプレートを使用する必要があります。
この種のサポートされているテンプレートの例は、App Services Integration iAppと呼ばれ、https://github.com/F5Networks/f5-application-services-integration-iAppで入手できます。

.. toctree::
   :maxdepth: 1
   :glob:

   lab*