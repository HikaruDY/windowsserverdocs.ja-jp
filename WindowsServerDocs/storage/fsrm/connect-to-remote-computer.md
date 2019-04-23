---
title: リモート コンピューターに接続する
description: この記事では、ファイル サーバー リソース マネージャーからリモート コンピューターに接続して、記憶域リソースを管理する方法について説明します。
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 93d2be926437b65ed8eb84a828ea0d7da6a51086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818863"
---
# <a name="connect-to-a-remote-computer"></a>リモート コンピューターに接続する 

> 適用対象:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

ファイル サーバー リソース マネージャーからコンピューターに接続して、リモート コンピューター上の記憶域リソースを管理できます。 接続すると、ファイル サーバー リソース マネージャーを使用して、クォータの管理、ファイルのスクリーン処理、分類の管理、ファイル管理タスクのスケジューリング、およびこれらのリモート リソースについてのレポート生成を実行できます。

> [!Note]
> ファイル サーバー リソース マネージャーでは、ローカル コンピューターまたはリモート コンピューター上のリソースを管理できますが、両方を同時に管理することはできません。

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>ファイル サーバー リソース マネージャーからリモート コンピューターに接続するには

1.  **[管理ツール]** の **[ファイル サーバー リソース マネージャー]** をクリックします。

2.  コンソール ツリーで、**[ファイル サーバー リソース マネージャー]** を右クリックし、**[別のコンピューターへ接続]** をクリックします。

3.  **[別のコンピューターへ接続]** ダイアログ ボックスで、**[別のコンピューター]** をクリックします。 接続先のサーバーの名前を入力します (または **[参照]** をクリックして、リモート コンピューターを検索します)。

4.  **[OK]** をクリックします。

> [!Important]
> **[別のコンピューターへ接続]** コマンドは、ファイル サーバー リソース マネージャーを **[管理ツール]** から開いた場合にのみ使用できます。 サーバー マネージャーからファイル サーバー リソース マネージャーにアクセスした場合は、このコマンドは使用できません。

## <a name="additional-considerations"></a>その他の考慮事項

ファイル リソース サーバー マネージャーを使用してリモート リソースを管理するには、次の条件を満たす必要があります。

-   リモート コンピューターの **Administrators** グループのメンバーであるドメイン アカウントを使用して、ローカル コンピューターにログオンする必要があります。
-   リモート コンピューターで Windows Server が実行され、ファイル サーバー リソース マネージャーがインストールされている必要があります。
-   リモート コンピューターで、**[リモート ファイル サーバー リソース マネージャー管理]** 例外を有効にする必要があります。 この例外は、[コントロール パネル] の [Windows ファイアウォール] で有効にします。

## <a name="see-also"></a>関連項目

-   [リモート記憶域リソースを管理します。](managing-remote-storage-resources.md)