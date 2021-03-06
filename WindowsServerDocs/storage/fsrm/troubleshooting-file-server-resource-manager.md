---
title: ファイル サーバー リソース マネージャーのトラブルシューティング
description: この記事では、ファイル サーバー リソース マネージャーの使用時によく発生する問題の解決方法について説明します。
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0f30bcfd07f28ecd3fa1618e4f5d89b7a0b4d14f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403087"
---
# <a name="troubleshooting-file-server-resource-manager"></a>ファイル サーバー リソース マネージャーのトラブルシューティング

> 適用対象:Windows Server 2019、Windows Server 2016、Windows Server (半期チャネル)、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

ここでは、ファイル サーバー リソース マネージャーの使用時によく発生する問題を紹介します。

> [!Note]
> トラブルシューティングを適切に行うには、ファイル サーバー リソース マネージャーによって生成されたイベント ログを調べる必要があります。 ファイル サーバー リソース マネージャーに関するイベント ログのエントリはすべて、ソース **SRMSVC** の **アプリケーション** イベント ログにあります。

## <a name="i-am-not-receiving-e-mail-notifications"></a>電子メール通知が届きません。

-   **原因**:電子メールのオプションが構成されていないか、正しく構成されていません。

-   **解決方法**: **[電子メールの通知]** タブの **[ファイルサーバーリソースマネージャーのオプション]** ダイアログボックスで、SMTP サーバーと既定の電子メールの受信者が指定されていて、有効であることを確認します。 テスト用の電子メールを送信して、情報が正しく、通知の送信に使用されている SMTP サーバーが正常に動作していることを確認します。 詳細については、「[電子メール通知を構成する](configure-email-notifications.md)」を参照してください。


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>電子メール通知をトリガーしたイベントが何回か続けて発生しましたが、受け取った電子メール通知は 1 通だけです。

-   **原因**:ブロックされているファイルを保存しようとしたとき、またはクォータのしきい値を超えたファイルを保存しようとしたときに、そのファイルのスクリーン処理やクォータイベントに対して電子メール通知が構成されている場合は、1つの電子メールのみが60分間に管理者に送信されます。 標準. これにより、管理者の電子メール アカウントに、重複するメッセージが多数送信されることが防止されます。

-   **解決方法**:**通知の制限** タブの **ファイルサーバーリソースマネージャーのオプション** ダイアログボックスで、電子メール、イベントログ、コマンド、および レポート の各通知の種類に制限時間を設定できます。 各制限では、同一の問題が発生したときに、同じ種類の通知が次に生成されるまで待機する期間を指定します。 詳細については、「[通知の制限を構成する](configure-notification-limits.md)」を参照してください。


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>記憶域レポートが失敗した状態が続いており、失敗の原因に関するイベント ログに情報がまったく、またはほとんどありません。

-   **原因**:レポートが保存されているボリュームが破損している可能性があります。
-   **解決方法**:ボリュームで**chkdsk**を実行してから、レポートの生成を再試行してください。

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>ファイル スクリーン処理の監査レポートに、情報が含まれていません。

-   **原因**:次の1つまたは複数の原因が考えられます。
    -   監査データベースが、ファイルのスクリーン処理の動作状況を記録するように構成されていない。
    -   監査データベースが空である (ファイルのスクリーン処理の動作状況が記録されていない)。
    -   ファイルのスクリーン処理の監査レポートのパラメーターが、監査データベースからデータを取得していない。
    
-   **解決方法**: **[ファイルスクリーンの監査]** タブの **[ファイルサーバーリソースマネージャーのオプション]** ダイアログボックスで、 **[監査データベースのファイルスクリーニング動作を記録]** する チェックボックスがオンになっていることを確認します。
    -   ファイル スクリーン処理の動作状況の記録の詳細については、「[ファイル スクリーンの監査を構成する](configure-file-screen-audit.md)」を参照してください。
    -   ファイル スクリーン処理の監査レポートの既定のパラメーターを構成するには、「[記憶域レポートを構成する](configure-storage-reports.md)」を参照してください。
    -   スケジュールされたレポート タスクまたはオン デマンド レポートのレポート パラメーターを編集するには、「[記憶域レポートの管理](storage-reports-management.md)」を参照してください。

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>作成した一部のクォータで、"使用済み" と "使用可能" の値が、実際の "制限" の設定に一致しません。

-   **原因**:入れ子になったクォータがある場合があります。この場合、サブフォルダーのクォータは、親フォルダーのクォータからより制限の厳しい制限を継承します。 たとえば、親フォルダーに 100 MB のクォータの制限が適用され、そのサブフォルダーのそれぞれに 200 MB のクォータが別途適用されている場合を考えてみましょう。 親フォルダーに合計 50 MBのデータ (サブフォルダーに格納されているデータの合計) が格納されている場合、各サブフォルダーの利用可能な容量は 50 MB と表示されます。

-   **解決方法**: **[クォータの管理]** で、 **[クォータ]** をクリックします。 **[結果]** ウィンドウで、トラブルシューティングを行うクォータ エントリを選択します。 **[操作]** ウィンドウで、 **[フォルダーに影響するクォータの表示]** をクリックし、親フォルダーに適用されているクォータを確認します。 これにより、選択したクォータよりも小さい記憶域制限が設定されている親フォルダーのクォータを特定できます。

