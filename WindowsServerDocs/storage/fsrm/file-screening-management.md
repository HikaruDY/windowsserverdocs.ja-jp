---
title: ファイル スクリーンの管理
description: この記事では、ファイル スクリーンの作成、通知の生成、ファイル スクリーン処理のテンプレートの定義、およびファイル スクリーンの例外の作成の方法について説明します。
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ac5032630f960329675f896a303ef197d6a4dbb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403109"
---
# <a name="file-screening-management"></a>ファイル スクリーンの管理

> 適用対象:Windows Server 2019、Windows Server 2016、Windows Server (半期チャネル)、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

ファイル サーバー リソース マネージャー MMC スナップインの **[ファイル スクリーンの管理]** ノードでは、次のタスクを実行できます。

-   ユーザーが保存できるファイルの種類を制御したり、承認されていないファイルの保存をユーザーが試みた際に通知を生成したりするため、ファイル スクリーンを作成します。
-   新しいボリュームまたはフォルダーに適用でき、組織全体で利用できる、ファイル スクリーン処理のテンプレートを定義します。
-   ファイル スクリーン処理の例外を作成し、ファイル スクリーン処理規則の柔軟性を広げます。

たとえば、次のようなことができます。

-   サーバー上の個人用フォルダーに音楽ファイルが格納されないように設定する一方で、法的権利管理をサポートする、または企業のポリシーに準拠している特定の種類のメディア ファイルの格納を許可できます。 同じ状況で、会社の副社長に対し、個人用フォルダーにすべての種類のファイルを格納できる特別な特権を与えることができます。
-   実行可能ファイルが共有フォルダーに格納された場合、ファイルを格納したユーザー、ファイルの正確な場所などの情報を含む通知を電子メールで受信できるようにスクリーン処理プロセスを実装できます。

ここでは、次のトピックについて説明します。

-   [スクリーン処理のためにファイル グループを定義する](define-file-groups-for-screening.md)
-   [ファイル スクリーンを作成する](create-file-screen.md)
-   [ファイル スクリーンの例外を作成する](create-file-screen-exception.md)
-   [ファイル スクリーン テンプレートを作成する](create-file-screen-template.md)
-   [ファイル スクリーンのテンプレートのプロパティを編集する](edit-file-screen-template-properties.md)

> [!Note]
> 電子メール通知や特定のレポート機能を設定するには、まずファイル サーバー リソース マネージャーの全般的なオプションを設定する必要があります。

## <a name="see-also"></a>関連項目

-   [ファイル サーバー リソース マネージャーのオプションを設定する](setting-file-server-resource-manager-options.md)


