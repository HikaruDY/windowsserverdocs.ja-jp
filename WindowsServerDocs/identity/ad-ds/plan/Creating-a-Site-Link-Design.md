---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: "サイト リンク設計を作成します。"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9eb54781035424c9a5210e11fbdeafc55496c6c3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-design"></a>サイト リンク設計を作成します。

>適用対象: Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

サイト リンクと、サイトを接続するサイト リンク設計を作成します。 サイト リンクは、サイト間の接続とレプリケーション トラフィックの転送に使用する方法を反映します。 各サイトでドメイン コントローラーは、Active Directory の変更をレプリケートできるようにサイト リンクにサイトを接続する必要があります。  
  
## <a name="connecting-sites-with-site-links"></a>サイト リンクにサイトを接続します。  
サイト リンクにサイトを接続するには、サイト リンクで接続、それぞれのサイト間トランスポート コンテナーにサイト リンク オブジェクトを作成し、サイト リンクを名前にするメンバーのサイトを識別します。 サイト リンクを作成した後、サイト リンクのプロパティを設定することができます。  
  
サイト リンクを作成する場合は、サイト リンクのすべてのサイトが含まれていることを確認します。 さらに、すべてのサイト接続されている相互に他のサイト リンクを通じて他のすべてのサイトに任意のサイト内のドメイン コントローラーからの変更をレプリケートできるようにを確認します。 これを行うに失敗した場合、イベント ビューアーでは、サイトのトポロジが接続されていないことを示すは、ディレクトリ サービス ログにエラー メッセージが生成されます。  
  
新しく作成されたサイト リンクにサイトを追加する場合は、常には、追加するサイトがその他のサイト リンクのメンバーであるかどうかを決定し、必要な場合は、サイトのサイト リンクのメンバーシップを変更します。 たとえばを行う場合、サイト、Default-First-Site-Link のメンバー最初にサイトを作成するときに、必ずを Default-First-Site-Link から新しいサイト リンクにサイトを追加した後、サイトを削除します。 Default-First-Site-Link からサイトを削除しない場合、知識整合性チェッカー (KCC) は、間違ったルーティングされる可能性があります、両方のサイト リンク、メンバーシップに基づくルーティングの決定を行います。  
  
サイト リンクによって接続するメンバーのサイトを識別するには、場所とは「地理的な場所との通信リンク」(DSSTOPO_1.doc) ワークシートに記録したリンク先の場所のリストを使用します。 複数のサイトには、同じ接続性と相互に可用性がある、同じサイト リンクを使用してそれらを接続できます。  
  
サイト間トランスポート コンテナーは、サイト リンクをリンクを使用するトランスポートにマップするための手段を提供します。 サイト リンク オブジェクトを作成する場合は、サイト リンクを IP トランスポート経由でリモート プロシージャ コール (RPC) に関連付けます、IP のコンテナーまたは SMTP トランスポートを使用して、サイト リンクに関連付けます簡易メール転送プロトコル (SMTP) のコンテナーで作成します。  
  
> [!NOTE]  
> SMTP レプリケーションは、Active Directory ドメイン サービス (AD DS); の将来のバージョンではサポートされません。そのため、SMTP コンテナーでのサイト リンク オブジェクトの作成は推奨されません。  
  
それぞれのサイト間トランスポート コンテナーにサイト リンク オブジェクトを作成するときに AD DS は、ドメイン コントローラー間でレプリケーションのレプリケーションとサイト間の両方を転送するのに IP 経由 RPC を使用します。 転送中にデータを安全に保つには、レプリケーションの IP を経由した RPC は、Kerberos 認証プロトコルとデータ暗号化の両方を使用します。  
  
直接 IP 接続が利用できない場合は、SMTP を使用してサイト間のレプリケーションを構成できます。 ただし、SMTP レプリケーションの機能は制限されており、エンタープライズ証明機関 (CA) が必要です。 SMTP は、構成、スキーマ、およびアプリケーション ディレクトリ パーティションをレプリケートすることができますのみとドメイン ディレクトリ パーティションのレプリケーションをサポートしていません。  
  
サイト リンクの名前、name_of_site1 name_of_site2 など、一貫性のある命名スキームを使用します。 サイト、リンク先のサイト、およびワークシートでこれらのサイトを接続するサイト リンクの名前の一覧を記録します。 サイト名と関連付けられているサイト リンクの名前を記録するためのワークシートで、ジョブ エイドの Windows Server 2003 導入ガイドを参照してください ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))、ダウンロード。Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip、および「サイトと関連付けられているサイト リンク」(DSSTOPO_5.doc) を開きます。  
  
## <a name="in-this-guide"></a>このガイドで  
[サイト リンクのプロパティの設定](Setting-Site-Link-Properties.md)  
  

