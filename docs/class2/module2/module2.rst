Module 2: 「f5-postman-workflows」フレームワーク & 「f5-newman-wrapper」ツールの紹介
==================================================

以前のモジュールでは、さまざまなレスポンス値のチェックや環境変数の設定に非常に面倒な作業がありました。 退屈であることに加えて、人間の判断をようする作業があるため自動化することはできません。

F5 BIG-IPプラットフォームの自動化を支援するため、Postman REST Clientで使用できる一連のツールを開発しました。
(http://getpostman.com).  このツールの目的は以下のとおりです。

- f5-postman-workflows

  - APIレスポンスのテストと環境変数の設定を容易にする、再利用可能なJavaScript関数を提供すること
  - 長時間実行操作をポーリングするメカニズムを実装すること

- f5-newman-wrapper

  - ユーザーがPostman Collectionをワークフローに簡単に組み込むことを可能にすること
  - Ansible、Chef＆Puppetなどのサードパーティのツールとの統合可能にすること

このフレームワークを使用すると、応答値のテスト、リクエスト・チェイニング用の環境変数の設定、および長時間実行APIプロセスが完了するまでのポーリング・メカニズムを含むコレクションを作成できます。

ユーザーは、Postman GUIクライアント、Postman Collection RunnerまたはNewman CLIクライアントを使用して、これらのコレクションを実行できます。

このモジュールは、ツールの使い方を説明します。 「f5-postman-workflows」フレームワークを使用してコレクションに興味がある場合は、公式のGitHubリポジトリをご覧ください。 https://github.com/0xHiteshPatel/f5-postman-workflows

.. toctree::
   :maxdepth: 1
   :glob:

   lab*
