Module 1 – REST APIの基礎とデバイスのオンボーディング
==============================================

このモジュールでは、BIG-IP iControl REST APIを利用するための必要な基本的な概念を学習します。
また、BIG-IPの冗長構成（Active/Standby）を実現する典型的なデバイスのオンボーディング・ワークフローについても説明します。 
ここでは、 **命令型** のアプローチを利用します。 

.. NOTE:: このラボには2台のBIG-IPが含まれていますが、割り当てられた時間内に全項目を完了できるように、ほとんどのラボではBIG-IP-Aというデバイスのみの設定に重点を置いています。(BIG-IP-Aの管理IP設定とライセンス・アクティベーションは既に完了しており、BIG-IP-Bにもいくつかの最小構成がロードされています。)　実環境では、すべてのBIG-IPでデバイスオンボーディング用の初期設定を実行する必要があります。

.. NOTE:: このラボで実行されたAPIコマンドの実行結果を確認するには、BIG-IPおよびiWorkflowデバイスに対して、HTTPS(GUI)またはSSH接続することで確認できます。また、トラブルシューティングやエラー原因を特定には以下のBIG-IPまたはiWorkflowのログを確認します。

    - BIG-IP:

      - /var/log/ltm
      
      - /var/log/restjavad.0.log

    - iWorkflow: 

      - /var/log/restjavad.0.log

.. NOTE::
    In order to confirm the results of REST API calls made in this lab, it's 
    beneficial to have GUI/SSH sessions open to BIG-IP and iWorkflow devices. 
    By default, BIG-IP and iWorkflow will log all REST API related events locally 
    to **restjavad.0.log** and can be configured to log to a remote syslog server 
    (see https://support.f5.com/csp/article/K13080 instructions). Also, the **ltm** 
    log file on BIG-IP will contain log messages that pertain specifically to 
    BIG-IP local traffic management events. These log file locations are below:

    - BIG-IP:

      - /var/log/ltm
      
      - /var/log/restjavad.0.log

    - iWorkflow: 

      - /var/log/restjavad.0.log

.. toctree::
   :maxdepth: 1
   :glob:

   lab*
