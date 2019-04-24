---
Title: Active Directory でクラスターのアカウントを構成する
ms.date: 11/12/2012
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 2fcc6047a0e85170754d8f05d10f728a4c529049
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871963"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>Active Directory でクラスターのアカウントを構成する


適用先:Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、および Windows Server 2008

Windows Server、フェールオーバー クラスターを作成して、クラスター化されたサービスまたはアプリケーションを構成する場合、フェールオーバー クラスター ウィザードが、必要な Active Directory コンピューター アカウント (コンピューター オブジェクトとも呼ばれます) を作成し、特定のアクセス許可を付与します。 ウィザードは、例外を HYPER-V 仮想マシン、クラスター自体のコンピューター アカウント (このアカウントとも、クラスター名オブジェクト CNO) とクラスター化されたサービスとアプリケーションのほとんどの種類のコンピューター アカウントを作成します。 これらのアカウントのアクセス許可は、フェールオーバー クラスター ウィザードによって自動的に設定されます。 アクセス許可を変更する場合は、クラスターの要件に一致するように戻す変更する必要があります。 このガイドは、これらの Active Directory アカウントとアクセス許可について説明しますには、重要な理由の背景を提供および構成し、アカウントを管理するための手順について説明します。
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>フェールオーバー クラスターで必要な Active Directory アカウントの概要

このセクションでは、(Active Directory コンピューター オブジェクトとも呼ばれます) がフェールオーバー クラスターのある重要な Active Directory コンピューター アカウントについて説明します。 これらのアカウントは次のとおりです。

  - **クラスターを作成するために使用するユーザー アカウント。** これは、クラスターの作成ウィザードを開始するために使用するユーザー アカウントです。 アカウントは、クラスター自体のコンピューター アカウントの作成元の基礎を提供するために重要です。  
      
  - **クラスター名アカウント。** (クラスター自体のコンピューター アカウントとも呼ばれます CNO、クラスター名オブジェクト)。 このアカウントは、クラスターの作成ウィザードによって自動的に作成され、クラスターと同じ名前を持ちます。 クラスター名アカウントは、このアカウントで他のアカウントが自動的に作成、クラスターで新しいサービスやアプリケーションを構成するために、非常に重要です。 クラスター名アカウントが削除された場合、またはアクセス許可がこれから取得されますとして他のアカウントを作成することはできませんは、クラスター名アカウントを復元または適切なアクセス許可が再開されるまで、クラスターで必要です。  
      
    たとえば、Cluster1 と呼ばれる、クラスターを作成して、クラスターで PrintServer1 と呼ばれるクラスター化されたプリント サーバーを構成しよう Active Directory で Cluster1 アカウントがコンピューターを作成するために使用できるように、適切なアクセス許可を保持する必要PrintServer1 をという名前のアカウント。  
      
    Active Directory 内のコンピューター アカウントの既定のコンテナーで、クラスター名アカウントが作成されます。 既定では、「コンピューター」コンテナーがドメイン管理者は、別のコンテナーへのリダイレクトに選択できますまたは組織単位 (OU)。  
      
  - **クラスター化されたサービスまたはアプリケーションのコンピューター アカウント (コンピューター オブジェクト)。** これらのアカウントは、ほとんどの種類のクラスター化されたサービスまたはアプリケーション、例外を HYPER-V 仮想マシンを作成するプロセスの一部として、高可用性ウィザードによって自動的に作成されます。 クラスター名アカウントは、これらのアカウントを制御するために必要なアクセス許可が付与されます。  
      
    たとえば、Cluster1 と呼ばれる、クラスターがあり、FileServer1 という名前のクラスター化されたファイル サーバーを作成し、高可用性ウィザードが FileServer1 と呼ばれる Active Directory コンピューター アカウントを作成します。 高可用性ウィザードは、FileServer1 アカウントを制御するために必要なアクセス許可 Cluster1 アカウントも提供します。  
      

次の表では、これらのアカウントに必要な権限について説明します。


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>アカウント</th>
<th>権限の詳細について</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>クラスターの作成に使用されるアカウント</p></td>
<td><p>クラスター ノードになるサーバーの管理アクセス許可が必要です。 必要です<strong>Create Computer objects</strong>と<strong>Read All Properties</strong>ドメイン内のコンピューター アカウントが使用されるコンテナーのアクセスを許可します。</p></td>
</tr>
<tr class="even">
<td><p>クラスター名アカウント (クラスター自体のコンピューター アカウント)</p></td>
<td><p>クラスターの作成ウィザードを実行すると、ドメイン内のコンピューター アカウントが使用される既定のコンテナーで、クラスター名アカウントを作成します。 既定では、(その他のコンピューター アカウント) のようなクラスター名アカウントは、ドメイン内、最大 10 台のコンピューター アカウントを作成できます。</p>
<p>クラスターを作成する前に、クラスター名アカウント (クラスター名オブジェクト) を作成する場合は、アカウントを事前設定 — 付ける必要があります、 <strong>Create Computer objects</strong>と<strong>Read All Properties</strong>ドメイン内のコンピューター アカウントが使用されるコンテナーのアクセスを許可します。 アカウントを無効にして、付与する必要があります<strong>フルコントロール</strong>のクラスターをインストールする管理者によって使用されるアカウントにします。 詳細については、次を参照してください。[クラスターを事前設定する手順は、アカウントを名前](#steps-for-prestaging-the-cluster-name-account)、このガイドで後述します。</p></td>
</tr>
<tr class="odd">
<td><p>クラスター化されたサービスまたはアプリケーションのコンピューター アカウント</p></td>
<td><p>高可用性ウィザードが実行 (新しいクラスター化されたサービスまたはアプリケーションを作成する)、ほとんどの場合、クラスター化されたサービスのコンピューター アカウントまたは Active Directory でアプリケーションを作成します。 クラスター名アカウントは、このアカウントを制御するために必要なアクセス許可が付与されます。 クラスター化された HYPER-V 仮想マシンは例外です。 このコンピューター アカウントは作成されません。</p>
<p>クラスター化されたサービスまたはアプリケーションのコンピューター アカウントを事前設定する場合は、必要なアクセス許可を構成する必要があります。 詳細については、次を参照してください。[クラスター化されたサービスまたはアプリケーションのアカウントを事前設定する手順](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)、このガイドで後述します。</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> Windows Server の以前のバージョンでクラスター サービスのアカウントが発生しました。 以降、Windows Server 2008、ただし、クラスター サービスで自動的に実行、特定のアクセス許可と、サービスに必要な特権を提供する特殊なコンテキスト (ローカル システムのコンテキストのような特権レベルが低いが)。 ただし、他のアカウントが必要なようにこのガイドで説明します。 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>フェールオーバー クラスタ リングでウィザードを使用するアカウントを作成する方法

次の図は、前のサブセクションで説明されているコンピューター アカウント (Active Directory オブジェクト) の作成と使用を示します。 これらのアカウントでは、管理者が、クラスターの作成ウィザードを実行し、し (クラスター化されたサービスまたはアプリケーションを構成する) を高可用性ウィザードを実行する場合が活躍します。

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

上記の図は、クラスターの作成ウィザードと、高可用性ウィザードを実行している 1 人の管理者に注意してください。 ただし、両方のアカウントが十分なアクセス許可がある場合は、2 つの別のユーザー アカウントを使用する 2 つの異なる管理者をられます。 アクセス許可は、要件に関連するフェールオーバー クラスター、Active Directory ドメイン、およびアカウントは、このガイドの後半で詳しく説明します。

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>クラスターで必要なアカウントが変更された場合に問題が発生する方法

次の図は、クラスターの作成ウィザードが自動的に作成した後、クラスター名アカウント (クラスターで必要なアカウントの 1 つ) が変更された場合は、問題が発生する方法を示します。

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

図に示す問題の種類が発生した場合、特定のイベント (1193、1194、1206、または 1207) は、イベント ビューアーに記録されます。 これらのイベントに関する詳細については、次を参照してください。 [ http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271)します。

(既定では 10) コンピューター オブジェクトを作成するため、ドメイン全体のクォータに達している場合、クラスター化されたサービスまたはアプリケーションのアカウントを作成すると、同様の問題が発生することができますに注意してください。 場合は、これは、ドメイン全体の設定と、慎重に検討した後のみと、前の図では使用できませんの確認後にのみ変更する必要があります。 が、クォータを増やす方法について、ドメイン管理者に相談する適切なあります。自分の状況について説明します。 詳細については、次を参照してください。[クラスターに関連する Active Directory アカウントの変更に起因する問題のトラブルシューティングの手順](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)、このガイドで後述します。

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>フェールオーバー クラスター、Active Directory ドメイン、およびアカウントに関連する要件

クラスター化されたサービスの前に、特定の要件を満たす必要があるように上記の 3 つのセクションで、およびアプリケーションをフェールオーバー クラスターに正常に構成できます。 最も基本的な要件は、(1 つのドメイン) 内のクラスター ノードの場所と、クラスターをインストールするユーザーのアカウントのアクセス許可のレベルに関するものです。 これらの要件が満たされた場合、クラスターで必要なその他のアカウントが、フェールオーバー クラスター ウィザードによって自動的に作成できます。 これらの基本的な要件の詳細を次に示します。

  - **ノード:** すべてのノードは、同じ Active Directory ドメイン内にする必要があります。 (ドメインは、Windows NT 4.0 は、Active Directory には含まれませんに基づくことはできません)。  
      
  - **クラスターをインストールしたユーザーのアカウント:** クラスターをインストールするユーザーは、次の特性を持つアカウントを使用する必要があります。  
      
      - アカウントはドメイン アカウントである必要があります。 これは、ドメイン管理者アカウントではありません。 この一覧にその他の要件を満たしている場合は、ドメイン ユーザー アカウントをられます。  
          
      - アカウントのクラスター ノードになるサーバーの管理アクセス許可が必要です。 これを実現する最も簡単な方法では、ドメイン ユーザー アカウントを作成し、そのアカウントを各クラスター ノードになるサーバー上でローカルの Administrators グループに追加します。 詳細については、次を参照してください。[クラスターをインストールするユーザーのアカウントを構成する手順](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)、このガイドで後述します。  
          
      - アカウント (または、アカウントのメンバーであるグループ) を指定する必要があります、 **Create Computer objects**と**Read All Properties**ドメイン内のコンピューター アカウントが使用されるコンテナーのアクセスを許可します。 別の方法として、アカウントにドメイン管理者アカウントを作成することです。 詳細については、次を参照してください。[クラスターをインストールするユーザーのアカウントを構成する手順](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)、このガイドで後述します。  
          
      - 組織は、クラスター名アカウント (クラスターと同じ名前のコンピューター アカウント) を事前設定が、事前設定されたクラスター名アカウントは、クラスターをインストールするユーザーのアカウントに「フル コントロール」アクセス許可を与える必要があります。 他の重要な詳細については、クラスター名アカウントをプレステージする方法について、次を参照してください。[クラスターを事前設定する手順は、アカウントを名前](#steps-for-prestaging-the-cluster-name-account)、このガイドで後述します。  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>パスワードのリセットやその他のアカウント メンテナンスの事前の計画

フェールオーバー クラスターの管理者は、クラスター名アカウントのパスワードをリセットする必要もあります。 このアクションには、特定のアクセス許可が必要です、**パスワードのリセット**権限。 そのため、ベスト プラクティス (Active Directory ユーザーとコンピューター スナップインを使用) して、クラスター名アカウントのアクセス許可を編集するのには、クラスターの管理者に提供する、**パスワードのリセット**クラスターのアクセス許可アカウントの名前。 詳細については、次を参照してください。[名アカウント クラスター パスワードに関する問題のトラブルシューティング手順](#steps_for_troubleshooting_password_problems_with_the_cluster_name_account)、このガイドで後述します。

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>クラスターをインストールするユーザーのアカウントを構成する手順

クラスターをインストールしたユーザーのアカウントは、クラスター自体のコンピューター アカウントの作成元の基盤を提供するために重要です。

次の手順を実行するために必要な最小のグループ メンバーシップに (他のユーザーによって作成された) アカウントにのみ配置するかどうか、またはドメイン アカウントを作成して、ドメインに必要なアクセス許可を割り当てることするかどうかによって異なります、。ローカル**管理者**フェールオーバー クラスター内のノードとなるサーバー上でグループ化します。 場合、前者のメンバーシップ**Account Operators**または**Domain Admins**、またはそれと同等がこの手順を実行するために必要な最低限です。 場合、ローカルで後者の場合、メンバーシップ**管理者**フェールオーバー クラスター内のノードとなるサーバー上でグループ化または必要なはそれと同等です。 適切なアカウントの使用に関する詳細を確認し、グループ メンバーシップ[ http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)します。

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>クラスターをインストールするユーザーのアカウントを構成するには

1.  作成またはクラスターをインストールしているユーザーのドメイン アカウントを取得します。 このアカウントは、ドメイン ユーザー アカウントまたはドメイン管理者アカウントを指定できます (で**Domain Admins**または同等のグループ)。

2.  かどうか作成または手順 1. で取得されたアカウントは、ドメイン ユーザー アカウントまたはドメイン内のドメイン管理者アカウントが自動的にローカルに含まれていない**管理者**ドメイン内のコンピューター グループに追加しますローカル アカウント**管理者**フェールオーバー クラスター内のノードとなるサーバー上でグループ化。
    
    1.  **[スタート]**、**[管理ツール]**、**[サーバー マネージャー]** の順にクリックします。  
          
    2.  コンソール ツリーで、展開**構成**、展開**ローカル ユーザーとグループ**、順に展開**グループ**します。  
          
    3.  中央のウィンドウで右クリック**管理者**、 をクリックして**をグループに追加**、 をクリックし、**追加**します。  
          
    4.  **を選択するオブジェクト名の入力**、手順 1. で作成または取得されたユーザー アカウントの名前を入力します。 メッセージが表示されたら、アカウント名とこのアクションのための十分なアクセス許可を持つパスワードを入力します。 **[OK]** をクリックします。  
          
    5.  フェールオーバー クラスター内のノードとなる各サーバーでこれらの手順を繰り返します。  
          
    

> [!IMPORTANT]
> クラスター内のノードとなるすべてのサーバー上では、次の手順を繰り返す必要があります。 
<br>


3.  作成または手順 1. で取得されたアカウントがドメイン管理者アカウントの場合は、この手順の残りの部分をスキップします。 それ以外の場合、アカウントに付与、 **Create Computer objects**と**Read All Properties**ドメイン内のコンピューター アカウントが使用されるコンテナーのアクセス許可。
    
    1.  ドメイン コント ローラーで、次のようにクリックします。**開始**、 をクリック**管理ツール**、 をクリックし、 **Active Directory ユーザーとコンピューター**します。 **[ユーザー アカウント制御]** ダイアログ ボックスが表示されたら、表示された操作が正しいことを確認し、**[続行]** をクリックします。  
          
    2.  **ビュー** ] メニューの [ことを確認します**高度な機能**が選択されています。  
          
        ときに**高度な機能**が選択されているを確認できます、**セキュリティ**アカウント (オブジェクト) のプロパティ タブで**Active Directory ユーザーとコンピューター**します。  
          
    3.  既定値を右クリックして**コンピューター**コンテナーまたは既定のコンテナーにコンピューター アカウントが、ドメインに作成され、順にクリックします**プロパティ**します。 **コンピューター**にある<b>Active Directory ユーザーとコンピューター/</b><i>ドメイン ノード</i><b>/コンピューター</b>します。  
          
    4.  **[セキュリティ]** タブで、 **[詳細設定]** をクリックします。  
          
    5.  クリックして**追加**、手順 1. で作成または取得されたアカウントの名前を入力し、 **OK**します。  
          
    6.  **のアクセス許可エントリ * * * コンテナー*  ダイアログ ボックスで、検索、 **Create Computer objects**と**Read All Properties** 、アクセス許可ことを確認し、 **許可**それぞれのチェック ボックスをオンします。  
          
        ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>クラスター名アカウントを事前設定する手順

代わりに、アカウントが作成され、クラスターの作成ウィザードを実行すると自動的に構成できるように、クラスター名アカウントを事前設定しない場合、通常は簡単です。 ただしを組織の要件により、クラスター名アカウントを事前設定する必要がある場合は、次の手順を使用します。

この手順を実行するには、**Domain Admins** グループのメンバーシップ、またはそれと同等のメンバーシップが最低限必要です。 適切なアカウントの使用に関する詳細を確認し、グループ メンバーシップ[ http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)します。 使用することできますと同じアカウントこの手順でクラスターを作成するときに使用するために注意してください。

#### <a name="to-prestage-a-cluster-name-account"></a>クラスター名アカウントをプレステージするには

1.  クラスターは必要がありますが、名前とクラスターを作成したユーザーが使用されるユーザー アカウントの名前がわかっていることを確認します。 (この手順を実行するアカウントを使用することができますに注意してください)。

2.  ドメイン コント ローラーで、次のようにクリックします。**開始**、 をクリック**管理ツール**、 をクリックし、 **Active Directory ユーザーとコンピューター**します。 **[ユーザー アカウント制御]** ダイアログ ボックスが表示されたら、表示された操作が正しいことを確認し、**[続行]** をクリックします。

3.  コンソール ツリーで、右クリック**コンピューター**または既定のコンテナーが、ドメイン内のどのコンピューターでアカウントを作成します。 **コンピューター**にある<b>Active Directory ユーザーとコンピューター/</b><i>ドメイン ノード</i><b>/コンピューター</b>します。

4.  クリックして**新規** をクリックし、**コンピューター**します。

5.  つまり、フェールオーバー クラスターに使用される名前は、クラスターの作成ウィザードで指定し、クラスター名を入力**OK**します。

6.  作成したアカウントを右クリックし、**アカウント無効にする**します。 選択を確認するメッセージが表示されたら、クリックして**はい**します。
    
    アカウントを無効にする必要がありますようにときに、**クラスターの作成**ウィザードを実行して、そのクラスターを使用するアカウントが現在、既存のコンピューターまたはドメインのクラスターで使用がないことを確認できます。

7.  **ビュー** ] メニューの [ことを確認します**高度な機能**が選択されています。
    
    ときに**高度な機能**が選択されているを確認できます、**セキュリティ**アカウント (オブジェクト) のプロパティ タブで**Active Directory ユーザーとコンピューター**します。

8.  クリックして、手順 3. で右クリックしたフォルダーを右クリックして**プロパティ**します。

9.  **[セキュリティ]** タブで、 **[詳細設定]** をクリックします。

10. をクリックして**追加**、 をクリックして**オブジェクトの種類**ことを確認し、**コンピューター**が選択されているし、をクリックし、 **ok**。 **を選択するオブジェクト名を入力**、作成したコンピューター アカウントの名前を入力し、クリックして**OK**。 メッセージが表示されている場合は、無効なオブジェクトを追加しようとしていることを示す**OK**します。

11. **アクセス許可エントリ** ダイアログ ボックスで、検索、**コンピューター オブジェクトを作成**と**Read All Properties**アクセス許可、ことを確認し、 **を許可します。** それぞれのチェック ボックスをオンします。
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. クリックして**OK**に戻るまで、 **Active Directory ユーザーとコンピューター**スナップイン。

13. クラスターの作成に使用されるため、この手順を実行するのと同じアカウントを使用する場合は、残りの手順をスキップします。 それ以外の場合、クラスターの作成に使用されるユーザー アカウントがある作成したコンピューター アカウントを完全に制御できるように、アクセス許可を構成する必要があります。
    
    1.  **ビュー** ] メニューの [ことを確認します**高度な機能**が選択されています。  
          
    2.  作成したコンピューター アカウントを右クリックし、**[プロパティ]** をクリックします。  
          
    3.  **[セキュリティ]** タブで、 **[追加]** をクリックします。 **[ユーザー アカウント制御]** ダイアログ ボックスが表示されたら、表示された操作が正しいことを確認し、**[続行]** をクリックします。  
          
    4.  使用して、 **[ユーザー、コンピューター、またはグループ**] ダイアログ ボックスで、クラスターを作成するときに使用されるユーザー アカウントを指定します。 **[OK]** をクリックします。  
          
    5.  追加したユーザー アカウントが選択されていることを確認し、**フル コントロール**を選択します、**許可**チェック ボックスをオンします。  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>クラスター化されたサービスまたはアプリケーションのアカウントを事前設定する手順

作成され、高可用性ウィザードを実行すると、自動的に構成されているアカウントを許可する代わりに、クラスター化されたサービスまたはアプリケーションのコンピューター アカウントを事前設定しない場合、通常は簡単です。 ただしを組織の要件のためのアカウントを事前設定する必要がある場合は、次の手順を使用します。

メンバーシップ、 **Account Operators**はこの手順を実行するために必要な最小のグループ、またはそれと同等です。 適切なアカウントの使用に関する詳細を確認し、グループ メンバーシップ[ http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)します。

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>クラスター化されたサービスまたはアプリケーションのアカウントをプレステージするには

1.  クラスターの名前と、クラスター化されたサービスまたはアプリケーションがある名前がわかっていることを確認します。

2.  ドメイン コント ローラーで、次のようにクリックします。**開始**、 をクリック**管理ツール**、 をクリックし、 **Active Directory ユーザーとコンピューター**します。 **[ユーザー アカウント制御]** ダイアログ ボックスが表示されたら、表示された操作が正しいことを確認し、**[続行]** をクリックします。

3.  コンソール ツリーで、右クリック**コンピューター**または既定のコンテナーが、ドメイン内のどのコンピューターでアカウントを作成します。 **コンピューター**にある<b>Active Directory ユーザーとコンピューター/</b><i>ドメイン ノード</i><b>/コンピューター</b>します。

4.  クリックして**新規** をクリックし、**コンピューター**します。

5.  クラスター化されたサービスやアプリケーションの使用は、クリックして名前を入力**OK**します。

6.  **ビュー** ] メニューの [ことを確認します**高度な機能**が選択されています。
    
    ときに**高度な機能**が選択されているを確認できます、**セキュリティ**アカウント (オブジェクト) のプロパティ タブで**Active Directory ユーザーとコンピューター**します。

7.  作成したコンピューター アカウントを右クリックし、**[プロパティ]** をクリックします。

8.  **[セキュリティ]** タブで、 **[追加]** をクリックします。

9.  をクリックして**オブジェクトの種類**ことを確認し、**コンピューター**が選択されているし、をクリックし、 **[ok]** します。 **を選択するオブジェクト名を入力**、クラスター名アカウントを入力し、クリックして**OK**。 メッセージが表示されている場合は、無効なオブジェクトを追加しようとしていることを示す**OK**します。

10. クラスター名アカウントが選択されていることを確認し、**フル コントロール**を選択します、**許可** チェック ボックス。
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>クラスターで使用されるアカウントに関連する問題のトラブルシューティングの手順

このガイドの前半で説明されていると、フェールオーバー クラスターを作成して、クラスター化されたサービスまたはアプリケーションを構成する場合は、フェールオーバー クラスター ウィザードは、必要な Active Directory アカウントを作成し、適切なアクセス許可を付与します。 必要なアカウントを削除すると、または必要なアクセス許可が変更された、問題が発生します。 次のサブセクションでは、これらの問題のトラブルシューティングの手順を提供します。

  - [クラスター名アカウントを使用してパスワードに関する問題のトラブルシューティングの手順](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [クラスターに関連する Active Directory アカウントの変更に起因する問題のトラブルシューティングの手順](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>クラスター名アカウントを使用してパスワードに関する問題のトラブルシューティングの手順

次のテキストを含むクラスター id、またはコンピューター オブジェクトに関するイベント メッセージが表示される場合は、この手順を使用します。 このテキストが、イベント メッセージの先頭ではなく、イベント メッセージ内になることに注意してください。

`Logon failure: unknown user name or bad password.`

前の説明に適合するイベント メッセージは、クラスター名アカウントのパスワードとクラスタ リング ソフトウェアによって格納されている対応するパスワードが不要になった一致を示します。

クラスター管理者に、必要に応じて、次の手順を実行する適切なアクセス許可があることを確認する方法については、計画をパスワードのリセットやこのガイドの前半で、他のアカウント メンテナンスの事前参照します。

この手順を完了するには、少なくともローカルの **Administrators** グループ、またはそれと同等のメンバーシップが必要です。 さらに、自分のアカウントを指定する必要があります**パスワードのリセット**、クラスター名アカウントに対するアクセス許可 (自分のアカウントがない限り、 **Domain Admins**アカウントか、クラスター名アカウントの Creator Owner)。 クラスターをインストールしたユーザーによって使用されたアカウントは、この手順で使用できます。 適切なアカウントの使用に関する詳細を確認し、グループ メンバーシップ[ http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)します。

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>クラスター名アカウントを使用してパスワードの問題をトラブルシューティングするには

1.  フェールオーバー クラスタ スナップインを開くには、**[スタート]** ボタンをクリックし、**[管理ツール]** をクリックします。次に、**[フェールオーバー クラスタ管理]** をクリックします。 (場合、**ユーザー アカウント制御** ダイアログ ボックスが表示されたら、いることを確認操作が表示をクリックして**続行**)。

2.  構成するクラスタがフェールオーバー クラスタの管理スナップインに表示されない場合は、コンソール ツリーで **[フェールオーバー クラスタ管理]** を右クリックして **[クラスタの管理]** をクリックし、目的のクラスタを選択または指定します。

3.  中央のウィンドウで **クラスター コア リソース**します。

4.  **クラスター名**を右クリックし、**名前**項目をポイントして、**その他のアクション**、順にクリックします**修復 Active Directory オブジェクト**します。

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>クラスターに関連する Active Directory アカウントの変更に起因する問題のトラブルシューティングの手順

新しいクラスター化されたサービスまたはアプリケーションを構成しようとすると、クラスター名アカウントが削除された場合、またはこれからのアクセス許可を取得する場合は、問題が発生します。 アカウントの名前とその他をこれが原因である、Active Directory ユーザーを使用する可能性があり、コンピューター スナップインを表示または変更、クラスターの問題のトラブルシューティングを行うには、アカウントに関連します。 この種の問題 (イベント 1193、1194、1206、または 1207) を参照してくださいの発生時にログに記録されるイベントについて[ http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271)します。

この手順を実行するには、**Domain Admins** グループのメンバーシップ、またはそれと同等のメンバーシップが最低限必要です。 適切なアカウントの使用に関する詳細を確認し、グループ メンバーシップ[ http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)します。

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>クラスターに関連する Active Directory アカウントの変更に起因する問題のトラブルシューティングを行う

1.  ドメイン コント ローラーで、次のようにクリックします。**開始**、 をクリック**管理ツール**、 をクリックし、 **Active Directory ユーザーとコンピューター**します。 **[ユーザー アカウント制御]** ダイアログ ボックスが表示されたら、表示された操作が正しいことを確認し、**[続行]** をクリックします。

2.  既定の展開**コンピューター**コンテナーまたはクラスター名アカウント (クラスターのコンピューター アカウント) が配置されているフォルダー。 **コンピューター**にある<b>Active Directory ユーザーとコンピューター/</b><i>ドメイン ノード</i><b>/コンピューター</b>します。

3.  クラスター名アカウントのアイコンを確認します。 下向きの矢印ボタンが必要ない、つまり、アカウントを無効にしないでします。 右クリックし、コマンドを確認を無効にする場合は、**アカウントの有効化**します。 コマンドを表示する場合は、それをクリックします。

4.  **ビュー** ] メニューの [ことを確認します**高度な機能**が選択されています。
    
    ときに**高度な機能**が選択されているを確認できます、**セキュリティ**アカウント (オブジェクト) のプロパティ タブで**Active Directory ユーザーとコンピューター**します。

5.  既定値を右クリックして**コンピューター**コンテナーまたは、クラスター名アカウントが配置されているフォルダー。

6.  **[プロパティ]** を選択します。

7.  **[セキュリティ]** タブで、 **[詳細設定]** をクリックします。

8.  、アクセス許可を持つアカウントの一覧で、クラスター名アカウントをクリックします。 クリックして**編集**します。
    

> [!NOTE]
> クラスター名アカウントが表示されない場合にクリックします<STRONG>追加</STRONG>し、一覧に追加します。 
<br>


9.  クラスター名アカウント (クラスター名オブジェクトまたは CNO とも呼ばれます) のことを確認します**許可**が選択されている、 **Create Computer objects**と**Read All Properties**アクセス許可。
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. クリックして**OK**に戻るまで、 **Active Directory ユーザーとコンピューター**スナップイン。

11. (該当する場合は、ドメイン管理者に相談) レビューのドメイン ポリシーは、コンピューター アカウント (オブジェクト) の作成に関連します。 クラスター名アカウントがクラスター化されたサービスまたはアプリケーションを構成するたびにコンピューター アカウントを作成できることを確認してください。 たとえば、ドメイン管理者が原因で、既定ではなく、特殊なコンテナーを作成するすべての新しいコンピューター アカウントの設定を構成**コンピューター**コンテナー、これらの設定を許可することを確認しますそのコンテナーに新しいコンピューター アカウントを作成するクラスター名アカウント。

12. 既定の展開**コンピューター**コンテナーまたはクラスター化されたサービスまたはアプリケーションのいずれかのコンピューター アカウントが配置されているコンテナー。

13. クラスター化されたサービスまたはアプリケーションのいずれかのコンピューター アカウントを右クリックし、をクリックし、**プロパティ**します。

14. **セキュリティ** タブで、アクセス許可があり、それを選択するアカウントの間で、クラスター名アカウントが表示されていることを確認します。 クラスター名アカウントがあることを確認します。**フル コントロール**権限 (、**許可** チェック ボックスが選択されている)。 そうでない場合、クラスター名アカウントを一覧に追加し、付けます**フルコントロール**権限。
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. 各クラスター化されたサービスと、クラスターで構成されたアプリケーションには、手順 13-14 を繰り返します。

16. (既定では 10) コンピューター オブジェクトを作成するため、ドメイン全体のクォータに達していない (該当する場合に、ドメイン管理者 consulting) を確認します。 この手順では、前の項目がすべてレビューおよび修正し、クォータに達している場合は、クォータを増やすことを検討します。 クォータを変更するには。
    
    1.  実行管理者としてコマンド プロンプトを開き**ADSIEdit.msc**します。  
          
    2.  右クリック**ADSI Edit**、 をクリックして**への接続**、順にクリックします**OK**します。 **既定の名前付けコンテキスト**コンソール ツリーに追加されます。  
          
    3.  ダブルクリック**既定の名前付けコンテキスト**、その下にあるドメインのオブジェクトを右クリックし、クリックして**プロパティ**します。  
          
    4.  スクロールして**ms-DS-MachineAccountQuota**選択し、をクリックして**編集**、値を変更し、クリックして**ok**します。
