---
title: 手順 4:Windows Server Essentials への移行のために設定とデータを移行先サーバーに移動する
description: Windows Server Essentials を使用する方法について説明します
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: fa6ab8e2108e569b7cef6bfbf0d20af4fa31016d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432569"
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>手順 4:Windows Server Essentials への移行のために設定とデータを移行先サーバーに移動する

>適用先:Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

ここでは、移行元サーバーからのデータと設定の移行について説明します。 移行先サーバーへの設定とデータの移動は次のように行います。  
  
-   [移行先サーバーにデータのコピー](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [ネットワークを構成します。](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [許可されているコンピューターのユーザー アカウントをマップします。](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a> 移行先サーバーにデータのコピー  
 移行元サーバーから移行先サーバーにデータをコピーする前に、以下のタスクを実行します。  
  
-   移行元サーバーで共有フォルダーのリストを確認します。各フォルダーのアクセス許可も含まれます。 移行元サーバーから移行するフォルダー構造に合わせて、移行先サーバーのフォルダーを作成またはカスタマイズします。  
  
-   各フォルダーのサイズを調べて、移行先サーバーに十分な記憶域があることを確認します。  
  
-   移行先サーバーにファイルをコピーしている間にドライブで書き込みが発生しないように、移行元サーバーの共有フォルダーをすべてのユーザーに対して読み取り専用にします。  
  
-   **[クライアント コンピューターのバックアップ]** フォルダーは移行先サーバーに移行できません。 サーバーの移行の前に、すべてのクライアント コンピューターが正常な状態であることを確認します。 サーバーの移行後に、すべての重要なクライアント コンピューターのデータがバックアップされるように、クライアント コンピューターのバックアップを構成して、起動することをお勧めします。  
  
-   **File History Backups**フォルダーは、Windows Server Essentials でのフォルダー構造とバックアップ メタデータの変更のため、移行先サーバーに直接移行することはできません。 ただし、特定のコンピューター上の特定のユーザーの **[ファイル履歴のバックアップ]** フォルダーを移行することは可能です。 そうするには、 **[ファイル履歴のバックアップ]** フォルダーで、そのユーザーとコンピューターの **[データ]** フォルダーを見つけて、その **[データ]** フォルダーを移行先サーバーの **[ファイル履歴のバックアップ]** フォルダーにコピーします。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>移行元サーバーから移行先サーバーにデータをコピーするには  
  
1. ドメイン管理者として移行先サーバーにサインインし、コマンド プロンプト ウィンドウまたは Windows PowerShell コマンド プロンプトを開きます。  
  
2. コマンド プロンプト ウィンドウを使用する場合は、次のコマンドを入力し、Enter キーを押します。  
  
   `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
    各項目の意味は次のとおりです。  
  
   - \<SourceServerName\>移行元サーバーの名前を指定します  
  
   - \<SharedSourceFolderName\>移行元サーバー上の共有フォルダーの名前を指定します  
  
   - \<PathOfTheDestination\>フォルダーを移動する絶対パスです  
  
   - \<SharedDestinationFolderName\>データをコピーする移行先サーバー上のフォルダーです  
  
     例:  `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`。  
  
3. Windows PowerShell を使用する場合は、次のコマンドを入力して、Enter キーを押します。  
  
    `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4. 移行元サーバーから移行する共有フォルダーごとに、このプロセスを繰り返します。  
  
##  <a name="BKMK_Network"></a> ネットワークを構成します。  
  
#### <a name="to-configure-the-network"></a>ネットワークを構成するには  
  
1. 移行先サーバーでダッシュボードを開きます。  
  
2. ダッシュボードの **[ホーム]** ページで、 **[セットアップ]** 、 **[Anywhere Access のセットアップ]** の順にクリックし、 **[クリックして Anywhere Access を構成する]** オプションを選択します。  
  
3. Anywhere Access のセットアップ ウィザードが表示されます。 ウィザードの指示に従って、ルーターとドメインの名前を構成します。  
  
   ルーターが UPnP フレームワークをサポートしていない場合、または UPnP フレームワークが無効になっている場合は、黄色の警告アイコンがルーター名の隣に表示されることがあります。 以下のポートが開かれていて、移行先サーバーの IP アドレスに向いていることを確認します。  
  
-   ポート 80:HTTP Web トラフィック  
  
-   ポート 443:HTTPS Web トラフィック  
  
> [!NOTE]
>  移行先サーバーで、パブリック ドメイン名を構成する場合は、移行元サーバーから、ドメイン名を解放し、動的 DNS 更新の競合を回避する必要があります。  
  
##  <a name="BKMK_MapPermittedComputers"></a> 許可されているコンピューターのユーザー アカウントをマップします。  
 以前のバージョンの Windows Small Business Server または Windows Server Essentials から移行される各ユーザー アカウントを、1 台または複数のコンピューターにマップする必要があります。  
  
#### <a name="to-map-user-accounts-to-computers"></a>コンピューターにユーザー アカウントをマップするには  
  
1.  Windows Server Essentials ダッシュ ボードを開きます。  
  
2.  ナビゲーション バーで、 **[ユーザー]** をクリックします。  
  
3.  ユーザー アカウントの一覧で、ユーザー アカウントを右クリックして、 **[アカウント プロパティの表示]** をクリックします。  
  
4.  **[Anywhere Access]** タブをクリックし、 **[リモート Web アクセスを許可し、Web サービス アプリケーションにアクセスする]** をクリックします。  
  
5.  **[共有フォルダー]** 、 **[コンピューター]** 、 **[ホームページ リンク]** の順にクリックし、 **[適用]** をクリックします。  
  
6.  **[コンピューター アクセス]** タブをクリックし、アクセスを許可するコンピューターの名前をクリックします。  
  
7.  各ユーザー アカウントについて、手順 3 ～ 6 を繰り返します。  
  
> [!NOTE]
>  クライアント コンピューターの構成を変更する必要はありません。 自動的に構成されます。  
  
> [!NOTE]
>  移行を完了した後、移行先サーバーで最初の新しいユーザー アカウントを作成するときに問題が発生する場合は、追加したユーザー アカウントを削除し、再度作成してください。  
  
## <a name="next-steps"></a>次のステップ  
 設定とデータを移行先サーバーに移動しました。 次に「[手順 5。Windows Server Essentials の移行先サーバーの移行でフォルダー リダイレクトを有効にする](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)します。  
  

すべての手順を表示するを参照してください。 [Windows Server Essentials への移行](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)します。

