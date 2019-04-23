---
title: winnt32
description: 'Windows コマンド」のトピック * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a0a6fb3-ba4e-4ace-8984-7f6d3875560e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28675ac6d5a1f1a7f56a9b72ef11a3e99e4ed130
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851263"
---
# <a name="winnt32"></a>winnt32

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server 2003 のインストールまたは成果物へのアップグレードを実行します。 実行することができます**winnt32** Windows Server 2003 で Windows 95、Windows 98、Windows Millennium edition、Windows NT、Windows 2000、Windows XP、または製品を実行するコンピューターでコマンド プロンプトからです。 実行すると **winnt32** Windows NT 4.0 を実行するコンピューター上の Service Pack 5 またはそれ以降を最初に適用する必要があります。
## <a name="syntax"></a>構文
```
winnt32 [/checkupgradeonly] [/cmd: <CommandLine>] [/cmdcons] [/copydir:{i386|ia64}\<FolderName>] [/copysource: <FolderName>] [/debug[<Level>]:[ <FileName>]] [/dudisable] [/duprepare: <pathName>] [/dushare: <pathName>] [/emsport:{com1|com2|usebiossettings|off}] [/emsbaudrate: <BaudRate>] [/m: <FolderName>]  [/makelocalsource] [/noreboot] [/s: <Sourcepath>] [/syspart: <DriveLetter>] [/tempdrive: <DriveLetter>] [/udf: <ID>[,<UDB_File>]] [/unattend[<Num>]:[ <AnswerFile>]]
```
### <a name="parameters"></a>パラメーター
|パラメーター|説明|
|-------|--------|
|必要があるなくなります|コンピューターが Windows Server 2003 の製品とアップグレードの互換性を確認します。<br /><br />このオプションを使用する場合 **/unattend**、ユーザー入力は必要ありません。  それ以外の場合、画面で、結果が表示され、それらを指定したファイル名で保存できます。 既定のファイル名は **upgrade.txt** フォルダーにします。|
|/cmd|セットアップの最後のフェーズの前に特定のコマンドを実行するように指示します。 これは、コンピューターを再起動し、セットアップが完了する前に、セットアップに必要な構成情報を収集した後が、後に発生します。|
|\<CommandLine >|セットアップの最後のフェーズの前に実行するコマンドラインを指定します。|
|/cmdcons|X86 ベースのコンピューターの場合は、スタートアップ オプションとして、回復コンソールをインストールします。  回復コンソールは、コマンド ライン インターフェイス (NTFS でフォーマットされたドライブを含む) ローカル ドライブへのアクセスの開始とサービスの停止などのタスクを実行することができます。 使用できるだけ、 **/cmdcons** オプション セットアップの完了後にします。|
|/copydir|オペレーティング システム ファイルがインストールされているフォルダー内の別のフォルダーを作成します。  たとえば、x86 と x64 ベースのコンピューターでは、作成できますと呼ばれるフォルダー *Private_drivers*インストール、およびフォルダーに配置済みのドライバー ファイル i386 元フォルダー内です。 型**/copydir:i386\\* * * Private_drivers*セットアップ新しくインストールされたコンピューターに新しいフォルダーの場所を作成するフォルダーをコピーするように**systemroot** \\*Private_drivers *。<br /><br />-   **i386** i386 の指定<br />-   **ia64** ia64 の指定<br /><br />使用する **/copydir** する数の他のフォルダーを作成します。|
|\<FolderName>|サイトの変更を保持するために作成したフォルダーを指定します。|
|/copysource|オペレーティング システム ファイルがインストールされているフォルダー内の追加の一時フォルダーを作成します。 使用する **/copysource** する数の他のフォルダーを作成します。<br /><br />フォルダーとは異なり **/copydir** 作成すると、 **/copysource** セットアップの完了後にフォルダーを削除します。|
|/debug|たとえば、レベルを指定すると、デバッグ ログを作成します **/debug4:Debug.log**します。  既定のログ ファイルは**C:\systemroot \winnt32.log**と|
|\<level>|レベル値と説明<br /><br />-   0:重大なエラー<br />-   1:エラー<br />-   2:既定のレベル。 警告<br />-   3:情報<br />-4: デバッグの詳細情報<br /><br />各レベルには、それ以下のレベルが含まれています。|
|dudisable/|動的更新実行されないようにします。 動的更新を実行しないセットアップは、元のセットアップ ファイルでのみ実行されます。 このオプションは、応答ファイルを使用し、そのファイルで動的更新のオプションを指定した場合でも、動的更新を無効になります。|
|前回|Windows Update の Web サイトからダウンロードする動的な更新ファイルを使用できるように、インストール共有の準備作業を実行します。 複数のクライアント用の Windows XP をインストールするため、この共有を使用し、ことができます。|
|\<pathName>|完全なパス名を指定します。|
|/dushare|Windows Update Web サイトから、以前実行するには、動的更新ファイル (セットアップで使用するために更新されたファイル) のダウンロード以前の共有を指定します **前回: * * * < pathName >* します。 クライアント上で実行する場合は、クライアントのインストールで使用することを指定で指定された共有上の更新ファイルの使用 <pathName>します。|
|/emsport:off|有効または、セットアップ中に、およびサーバーのオペレーティング システムがインストールされた後に、緊急管理サービスを無効にします。 緊急管理サービスと緊急の場合にローカルのキーボード、マウス、およびネットワークが利用できない場合などのモニターが必要となる通常のサーバーをリモートで管理できます。 またはサーバーが正しく機能していません。 緊急管理サービスは、特定のハードウェア要件を持ち、Windows Server 2003 の製品に対してのみ使用できますがあります。<br /><br />-   **com1**は x86 ベースのコンピューター (いない Itanium アーキテクチャ ベースのコンピューターにのみ適用されます。<br />-   **com2**は x86 ベースのコンピューター (いない Itanium アーキテクチャ ベースのコンピューターにのみ適用されます。<br />-既定値です。 BIOS シリアル ポート Console Redirection (SPCR) テーブル内または Itanium アーキテクチャ ベース システムで、EFI コンソール デバイス パスで指定された設定を使用します。 指定した場合 **usebiossettings** SPCR テーブルと EFI コンソール デバイスの適切なパスはありません、緊急管理サービスは有効になりません。<br />-   **オフ**緊急管理サービスを無効にします。 後で、ブート設定を変更することで有効にできます。|
|/emsbaudrate|x86 ベースのコンピューターには、緊急管理サービスのボー レートを指定します。 (オプションは Itanium アーキテクチャ ベースのコンピューターに適用されません) です。使用する必要があります **/emsport:com1** または **/emsport:com2** (それ以外の場合、  **/emsbaudrate** は無視されます)。|
|\<BaudRate>|9600 ボー レートを示す 19200、57600、または 115200 です。 既定値は 9600 です。|
|/m|セットアップでは、別の場所から置換ファイルをコピーを指定します。  セットアップを最初に、別の場所に検索し、ファイルを見つけた場合、既定の場所からファイルの代わりに使用するように指示します。|
|/makelocalsource|セットアップ キーを押して、すべてのインストール ソース ファイルをローカル ハード_ディスクにコピーします。  使用 **/makelocalsource** cd が、インストールの後で利用できない場合は、インストール ファイルを提供する cd からインストールする場合。|
|/noreboot|セットアップ キーを押して、別のコマンドを実行できるように、セットアップのファイルのコピー フェーズが完了した後、コンピューターを再起動します。|
|/s|インストールするファイルのソースの場所を指定します。 同時に複数のサーバーからファイルをコピーするには、入力、 **/s:**\<Sourcepath > オプションを複数回 (最大 8 個)。 オプションを複数回使用する場合は、指定された最初のサーバーが必要になります。 または、セットアップは失敗します。|
|\<Sourcepath >|完全なソース パス名を指定します。|
|/syspart|X86 ベースのコンピューターの場合を指定します。 ことができますセットアップ スタートアップ ファイルをハード_ディスクにコピー、アクティブとしてマークし、ディスクを別のコンピューターにインストールします。 コンピューターを起動すると、セットアップの次のフェーズで自動的に開始します。<br /><br />常に使用する必要があります、 **/tempdrive** を持つパラメーター、 **/syspart** パラメーター。<br /><br />開始することができます**winnt32**で、 **/syspart**オプションで Windows Server 2003、Windows NT 4.0、Windows 2000、Windows XP、または製品を実行している x86 ベースのコンピューター。 コンピューターが Windows NT 4.0 を実行している場合は、Service Pack 5 以降が必要です。 Windows 95、Windows 98、または Windows Millennium edition は、コンピューターを実行することはできません。|
|\<ドライブ文字 >|ドライブ文字を指定します。|
|/tempdrive|指定したパーティションに一時ファイルの配置がセットアップに指示します。<br /><br />新規インストールでは、指定したパーティションにサーバーのオペレーティング システムもインストールされます。<br /><br />アップグレードの場合、 **/tempdrive**オプションに影響する一時ファイルのみの配置は、実行元のパーティションでオペレーティング システムがアップグレードされる**winnt32**します。|
|/udf|識別子を示します (\<ID >)、セットアップを使用して、一意性 Database (UDB) ファイルが、応答ファイルを変更する方法を指定 (を参照してください、 **/unattend**オプション)。  UDB では、応答ファイル内の値を上書きします。 し、識別子が使用する UDB ファイル内の値を決定します。 たとえば、 **/udf:RAS_user,Our_company.udb** Our_company.udb ファイル内に RAS_user 識別子に指定した設定をオーバーライドします。 ない場合は\<< > を指定すると、セットアップが、ユーザーを含むディスクを挿入するように指示されたら、 **$unique$.udb$ $unique$.udb**ファイル。|
|\<ID>|一意性 Database (UDB) ファイルが、応答ファイルを変更する方法を指定するための識別子を示します。|
|\<UDB_file>|一意性 Database (UDB) ファイルを指定します。|
|/unattend|X86 ベースのコンピューターの場合は、無人セットアップ モードでの Windows NT 4.0 Server (Service Pack 5 以降) または Windows 2000 以前のバージョンにアップグレードします。 すべてのユーザー設定は、セットアップ中に必要なユーザーの介入がないために、以前のインストールから取得されます。|
|\<num>|ファイル、コンピューターを再起動したときのコピーが終了をセットアップするまでの時間の秒数を指定します。 使用することができます\<Num > Windows Server 2003、Windows 98、Windows Millennium edition、Windows NT、Windows 2000、Windows XP、または製品を実行するコンピューターでします。 コンピューターが Windows NT 4.0 を実行している場合は、Service Pack 5 以降が必要です。|
|\<AnswerFile>|セットアップに、独自の仕様は、します。|
|/?|コマンド プロンプトにヘルプを表示します。|

## <a name="remarks"></a>注釈
クライアント コンピューターで Windows XP を展開する場合は、Windows XP に付属している winnt32.exe のバージョンを使用することができます。 Windows XP を展開する別の方法では、winnt32.msi、テクノロジの Windows インストーラー、IntelliMirror の一部を動作の設定を使用します。 クライアントの展開の詳細については、「Windows Server 2003 導入ガイドを参照してください。 [Windows 導入ガイドとリソース キットを使用して](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx)します。

Itanium ベースのコンピューターの場合は、 **winnt32** から拡張ファームウェア インターフェイス (EFI) または Windows Server 2003 Enterprise、Windows Server 2003 R2 Enterprise、Windows Server 2003 R2 Datacenter、または Windows Server 2003 Datacenter から実行できます。 Itanium アーキテクチャ ベースのコンピューターにも、 **/cmdcons** と **/syspart** が表示されていないとアップグレードに関連するオプションは使用できません。
ハードウェアの互換性の詳細については、次を参照してください。[ハードウェアの互換性](https://technet.microsoft.com/library/cc757927(v=ws.10).aspx)します。
動的更新を使用して、複数のクライアントのインストールに関する情報の詳細は、「Windows Server 2003 導入ガイドを参照してください。 [、Windows 導入ガイドとリソース キットを使用して](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx)います。
ブート設定を変更する方法の詳細については、Windows の展開と Windows Server 2003 のリソース キットを参照してください。 詳細については、次を参照してください。 [Windows 導入ガイドとリソース キットを使用して](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx)します。
使用して、 **/unattend** コマンド ライン オプションの設定を自動化するものが読み取りを Microsoft ライセンス契約の Windows Server 2003 とみなされます。 このコマンド ライン オプションを使用して、組織の Windows Server 2003 をインストールする、前に、ことを確認する必要がありますエンド ユーザー (個人、または 1 つのエンティティ) かどうかが受信した、読み取り、およびその製品の Microsoft ライセンス契約の条件に同意します。  Oem は、エンドユーザーが使用しているコンピューター上のこのキーを指定できない場合があります。

## <a name="additional-references"></a>その他の参照情報
-   [コマンドライン構文キー](command-line-syntax-key.md)
