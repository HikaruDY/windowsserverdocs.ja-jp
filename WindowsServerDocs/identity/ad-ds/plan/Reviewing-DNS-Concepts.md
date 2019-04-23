---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: "DNS の概念を確認します。"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 29c4c3d1d785d1b3b1fa1926eca8298aab2b3ee3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-dns-concepts"></a>DNS の概念を確認します。

>適用対象: Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

ドメイン ネーム システム (DNS) は、名前空間を表す分散データベースです。 名前空間には、すべてのクライアントは、任意の名前を検索するために必要な情報が含まれます。 任意の DNS サーバーは、その名前空間内の任意の名前に関するクエリに応答できます。 DNS サーバーでは、次の方法のいずれかでクエリがお答えします。  
  
-   答えがキャッシュ内にある場合は、キャッシュからクエリが応答します。  
  
-   回答は、DNS サーバーによってホストされているゾーンでは、そのゾーンからクエリが応答します。 ゾーンは、DNS サーバーに保存されている DNS ツリーの一部です。 ゾーンをホストする DNS サーバー、ときに、そのゾーンの名前に対する権限を持つ (つまり、DNS サーバー クエリに応答できます、ゾーン内の任意の名前の)。 たとえば、contoso.com ゾーンをホストしているサーバー contoso.com で任意の名前に対するクエリに応答できます。  
  
-   サーバーは、そのキャッシュまたはゾーンからクエリに応答できません、答えを他のサーバーを照会します。  
  
Active Directory 論理構造の設計に直接影響があるため、DNS の委任、再帰的名前解決、および Active Directory 統合 DNS ゾーンなどのコア機能を理解する必要があります。  
  
DNS と Active Directory ドメイン サービス (AD DS) の詳細については、次を参照してください。[DNS と AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)します。  
  
## <a name="delegation"></a>委任  
任意の名前に関するクエリに応答する DNS サーバーの場合、名前空間内のすべてのゾーンに直接的または間接的なパスが必要です。 これらのパスは、委任を使用して作成されます。 委任は、階層の次のレベルのゾーンの権限を持つネーム サーバーを一覧表示する親ゾーンにレコードです。 委任する他のゾーン内のサーバーにクライアントを参照する 1 つのゾーン内のサーバー。 次の図は、委任の 1 つの例を示します。  
  
![DNS の概念](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
DNS ルート サーバー ドットで表されるルート ゾーンのホスト (します。 ). ルート ゾーンには、次のレベルの com のゾーン、階層内のゾーンへの委任が含まれています。 ルート ゾーンの委任は、com ゾーンを見つけるに問い合わせる必要があります、Com サーバーは DNS ルート サーバーを示します。 同様に、com ゾーンで委任 Com をサーバーに通知するには、contoso.com ゾーンを見つけるに問い合わせる必要があります、Contoso サーバー。  
  
> [!NOTE]  
> 委任は、2 種類のレコードを使用します。 ネーム サーバー (NS) リソース レコードは、権限のあるサーバーの名前を提供します。 ホスト (A) とホスト (AAAA) リソース レコード アドレスを指定 IP バージョン 4 (IPv4) と IP version 6 (IPv6) 権限を持つサーバーです。  
  
このゾーンと委任のシステムでは、DNS 名前空間を表す階層ツリーを作成します。 各ゾーンは、階層内のレイヤーを表し、各委任は、ツリーの分岐を表します。  
  
ゾーンと委任の階層を使用すると、DNS ルート サーバーは DNS 名前空間で任意の名前を検索できます。 ルート ゾーンには、階層内の他のすべてのゾーンを直接的または間接的をリードする委任が含まれています。 DNS ルート サーバーを照会できる任意のサーバーは、名前空間内の任意の名前を検索するのに、委任で情報を使用できます。  
  
## <a name="recursive-name-resolution"></a>再帰的名前解決  
再帰的名前解決は、DNS サーバーが対象権限がないクエリに応答するゾーンと委任の階層を使用するプロセスです。  
  
いくつかの構成では、DNS サーバーには、DNS ルート サーバーを照会してそれを実現するルート ヒント (つまり、名と IP アドレスのリスト) が含まれます。 その他の構成では、サーバーは、別のサーバーに応答できないすべてのクエリを転送します。 転送やルート ヒントは、両方のためにはない権限のあるクエリを解決するのには DNS サーバーが使用できる方法です。  
  
### <a name="resolving-names-by-using-root-hints"></a>ルート ヒントを使用して名前の解決  
ルート ヒントには、DNS ルート サーバーを検索する任意の DNS サーバーが有効にします。 DNS サーバーは DNS ルート サーバーを見つけると、後にその名前空間のすべてのクエリを解決できます。 次の図は、DNS がルート ヒントを使用して名前を解決する方法について説明します。  
  
![DNS の概念](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
この例では、次のイベントが発生します。  
  
1.  クライアントは、再帰クエリを ftp.contoso.com 名に対応する IP アドレスを要求する DNS サーバーに送信します。 再帰クエリでは、クライアントが、クエリに明確な回答を依頼することを示します。 再帰クエリへの応答には、有効なアドレスまたはアドレスが見つからないことを示すメッセージがあります。  
  
2.  DNS サーバーが名前に対して権限がない、キャッシュに、応答していないため、DNS サーバーは DNS ルート サーバーの IP アドレスを検索するのにルート ヒントを使用します。  
  
3.  DNS サーバーは、反復的なクエリを使用して、ftp.contoso.com 名を解決するのには DNS ルート サーバーに要求します。 反復的なクエリでは、サーバーがクエリに明確な回答の代わりに別のサーバーへの紹介を受け入れられることを示します。 ftp.contoso.com 名前がラベルで終わるため、com DNS ルート サーバーで、com ゾーンをホストする Com サーバーに参照が返されます。  
  
4.  DNS サーバーは、反復的なクエリを使用して、ftp.contoso.com 名を解決するのには、Com サーバーします。 ftp.contoso.com 名は名前 contoso.com で終わる、ため Com サーバー参照が返されます、Contoso サーバーにそのゾーンのホスト、contoso.com します。  
  
5.  DNS サーバーは、反復的なクエリを使用して、ftp.contoso.com 名を解決するのには、Contoso サーバーです。 Contoso のサーバーは、ゾーン データ内の回答を検索し、サーバーへの応答を返します。  
  
6.  サーバーは、クライアントに、結果を返します。  
  
### <a name="resolving-names-by-using-forwarding"></a>転送を使用して名前の解決  
転送では、ルート ヒントを使用する代わりに特定のサーバーからルート名解決できます。 次の図は、DNS が転送を使用して名前を解決する方法について説明します。  
  
![DNS の概念](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
この例では、次のイベントが発生します。  
  
1.  クライアントは、名前 ftp.contoso.com 用の DNS サーバーを照会します。  
  
2.  DNS サーバーは、フォワーダーと呼ばれる別の DNS サーバーにクエリを転送します。  
  
3.  フォワーダーは名前に対して権限がありません、キャッシュに、応答していないため、DNS ルート サーバーの IP アドレスを検索するのにルート ヒントを使用します。  
  
4.  フォワーダは、反復的なクエリを使用して、ftp.contoso.com 名を解決するのには DNS ルート サーバーに要求します。 ftp.contoso.com 名は名前で終わるため、com DNS ルート サーバーで、com ゾーンをホストする Com サーバーに参照が返されます。  
  
5.  フォワーダーは、反復的なクエリを使用して、ftp.contoso.com 名を解決するのには、Com サーバーします。 ftp.contoso.com 名は名前 contoso.com で終わる、ため Com サーバー参照が返されます、Contoso サーバーにそのゾーンのホスト、contoso.com します。  
  
6.  フォワーダーは、反復的なクエリを使用して、ftp.contoso.com 名を解決するのには、Contoso サーバーです。 Contoso サーバーは、そのゾーン ファイル内の回答を検索し、サーバーへの応答を返します。  
  
7.  フォワーダーは、元の DNS サーバーに、結果を返します。  
  
8.  元の DNS サーバーは、クライアントに、結果を返します。  
  

