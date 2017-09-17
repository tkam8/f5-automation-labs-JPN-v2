Module 2 – iWorkflow
====================

このモジュールでは、F5のiWorkflowプラットフォームを使用してアプリケーションサービスを抽象化し、これらのサービスをテナントに提供する方法について学びます。
iWorkflowは、Automation＆Orchestrationにおいて2つの主な目的を持っています。

-  簡素化されたカスタマイズ可能なデバイスオンボーディングのワークフローを提供
-  L4-L7サービスデリバリ用のテナント/プロバイダインタフェースを提供

iWorkflowベースのツールチェーンに移行すると、L1-3の自動化(Device Onboarding、Networking等)とL4-7の自動化(Virtual Serverの設定、Poolの設定等)が分離され、それぞれ異なる機能によって提供されます。

L4-7サービスは、次のいずれかのモデルによって提供されます。

-  宣言型(Declarative)モデル：BIG-IPデバイス上のF5 iAppテンプレートを利用して、サービスカタログを作成
-  命令型(Imperative)モデル ：iWorkflow RESTプロキシを利用して、BIG-IPデバイス上のAPIを呼び出し

このラボでは、L1-7の完全自動化を実現するための高度な機能に焦点を当てます。L4-7サービスには、ツールチェーンの主要コンポーネントのiAppのひとつである ``f5.http`` を使用して、シンプルな構成を構築します。
より高度なユースケースの場合は、``Declarative`` または ``Deployment-centric`` （デプロイメント中心)のiAppテンプレートを使用する必要があります。
F5サポートのテンプレート（Application Services Integration iApp）は、https://github.com/F5Networks/f5-application-services-integration-iApp で入手できます。

.. toctree::
   :maxdepth: 1
   :glob:

   lab*
