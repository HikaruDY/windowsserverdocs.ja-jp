---
title: PEAP と EAP の要件の証明書テンプレートを構成します。
description: このトピックでは、ネットワーク ポリシー サーバーと Windows Server 2016 でのリモート アクセスで証明書の使用に関する情報を提供します。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e597a65982aeead907c9a41f17f1785a0bf81016
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>PEAP と EAP の要件の証明書テンプレートを構成します。

>適用対象: Windows Server (半期チャネル)、Windows Server 2016

拡張認証 Protocol\-トランスポート層セキュリティ \(EAP\-TLS\)、Extensible Authentication Protocol\-トランスポート層の保護されているセキュリティ \(PEAP\-TLS\)、PEAP\ Microsoft チャレンジ ハンドシェイク認証プロトコル バージョン 2 とネットワーク アクセスの認証に使用される証明書をすべて \ (MS\ CHAP v2\) は、X.509 証明書の要件および Secure Socket Layer/トランスポート レベルのセキュリティ (SSL/TLS) を使用する接続の作業を満たす必要があります。 クライアントとサーバーの両方の証明書には、追加の要件があります。

>[!IMPORTANT]
>このトピックでは、証明書テンプレートを構成するための手順を説明します。 使用するには次の手順が必要な Active Directory Certificate Services \(AD CS\) で独自の公開キー基盤 \(PKI\) が展開されていること。

## <a name="minimum-server-certificate-requirements"></a>最小サーバー証明書の要件

PEAP\ MS\-CHAP v2、PEAP\-TLS または EAP\-TLS 認証方法として、NPS サーバーは、最小サーバー証明書の要件を満たしているサーバー証明書を使用する必要があります。 

使用してサーバーの証明書を検証するクライアント コンピューターを構成することができます、**サーバーの証明書**オプション、クライアント コンピューターまたはグループ ポリシーでします。 

サーバー証明書は、次の要件を満たしている場合、クライアント コンピューターは、サーバーの認証の試行を受け取ります。

- サブジェクト名には、値が含まれています。 空のサブジェクト名がネットワーク ポリシー サーバー (NPS) を実行しているサーバーに証明書を発行した場合、証明書がありません NPS サーバーの認証にします。 サブジェクト名を持つ証明書テンプレートの構成。

    1. 証明書テンプレートを開きます。
    2. 詳細ウィンドウで、証明書テンプレートを右クリックを変更して、をクリックする**プロパティ**します。
    3. をクリックして、**サブジェクト名**タブをクリックし、をクリックして**Active Directory の情報から構築**します。
    4. **サブジェクト名の形式**、以外の値を選択**None**します。

- コンピューター証明書が、サーバーにチェーンする信頼されたルート証明機関 (CA) とは、リモート アクセス ポリシーまたはネットワーク ポリシーで指定される CryptoAPI によって実行されるチェックのいずれかが失敗します。

- NPS サーバーまたは VPN サーバーのコンピューター証明書は、拡張キー使用法 (EKU) 拡張機能でサーバー認証の目的で構成されます。 (サーバー認証用オブジェクト識別子は 1.3.6.1.5.5.7.3.1 です)。

- 必要なアルゴリズムの値をサーバー証明書が構成されている**RSA**します。 必須の暗号化設定を構成するには。

    1. 証明書テンプレートを開きます。
    2. 詳細ウィンドウで、証明書テンプレートを右クリックを変更して、をクリックする**プロパティ**します。
    3. をクリックして、**暗号化**] タブ。**アルゴリズム名**、] をクリックして**RSA**します。 いることを確認**最小キー サイズ**に設定されている**2048**します。

- サブジェクトの別名 (SubjectAltName) 拡張機能を使用する場合が、サーバーの DNS 名を含める必要があります。 登録サーバーのドメイン ネーム システム (DNS) 名では、証明書テンプレートを構成します。 

    1. 証明書テンプレートを開きます。
    2. 詳細ウィンドウで、証明書テンプレートを右クリックを変更して、をクリックする**プロパティ**します。
    3. をクリックして、**サブジェクト名**タブをクリックし、をクリックして**Active Directory の情報から構築**します。
    4. **代わりのサブジェクト名にこの情報を含める**[ **DNS 名**します。

PEAP と EAP-TLS を使用する場合、NPS サーバーはコンピューター証明書ストアの次の例外がインストールされているすべての証明書の一覧を表示します。

- EKU 拡張機能でサーバー認証の目的を含まない証明書は表示されません。

- 証明書サブジェクト名が含まれていないは表示されません。

- レジストリ ベースとスマート カード ログオン証明書は表示されません。

詳細については、次を参照してください。[802.1 X ワイヤードおよびワイヤレス展開用のサーバー証明書の展開](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)します。

## <a name="minimum-client-certificate-requirements"></a>最小限のクライアント証明書の要件

EAP-TLS または PEAP-TLS を使用して、証明書は、次の要件を満たしている場合、サーバーはクライアント認証の試行を受け入れます。

- クライアント証明書は、エンタープライズ CA によって発行されたまたは Active Directory Domain Services \(AD DS\) でユーザーまたはコンピューター アカウントにマップします。

- 信頼されたルート CA では、クライアントのチェーンに証明書ユーザーまたはコンピューターにはの EKU 拡張機能でクライアント認証の目的が含まれています \ (クライアント認証用オブジェクト識別子が 1.3.6.1.5.5.7.3.2\)、CryptoAPI によって実行されると、リモート アクセス ポリシーまたはネットワーク ポリシーで指定されているチェックも NPS ネットワーク ポリシーで指定されている証明書のオブジェクト識別子のチェックが失敗したとします。

- いずれかのスマート カード ログオンはレジストリ ベースの証明書または証明書のパスワードで保護されている、802.1 X クライアントは使用しません。

- ユーザー証明書を証明書のサブジェクトの別名 \(SubjectAltName\) 拡張機能に含まれるユーザー プリンシパル名 \(UPN\) します。 UPN の証明書テンプレートの構成。

    1. 証明書テンプレートを開きます。
    2. 詳細ウィンドウで、証明書テンプレートを右クリックを変更して、をクリックする**プロパティ**します。
    3. をクリックして、**サブジェクト名**タブをクリックし、をクリックして**Active Directory の情報から構築**します。
    4. **代わりのサブジェクト名にこの情報を含める**を選択**ユーザー プリンシパル名 \(UPN\)**します。

- コンピューター証明書は、証明書のサブジェクトの別名 \(SubjectAltName\) 拡張機能が完全修飾ドメインを含める必要があります \(FQDN\) とも呼ばれると、クライアントの名前、*DNS 名*します。 証明書テンプレートでこの名前の構成。

    1. 証明書テンプレートを開きます。
    2. 詳細ウィンドウで、証明書テンプレートを右クリックを変更して、をクリックする**プロパティ**します。
    3. をクリックして、**サブジェクト名**タブをクリックし、をクリックして**Active Directory の情報から構築**します。
    4. **代わりのサブジェクト名にこの情報を含める**[ **DNS 名**します。

PEAP\ TLS と EAP\ TLS では、クライアントは、証明書スナップインで、次の例外がインストールされているすべての証明書の一覧を表示します。

- ワイヤレス クライアントを対応するレジストリ ベース表示しないとスマート カード ログオン証明書。 

- クライアントのワイヤレスおよび VPN クライアントでは、パスワードで保護された証明書は表示されません。 

- EKU 拡張機能でクライアント認証の目的を含まない証明書は表示されません。


NPS の詳細については、次を参照してください。[ネットワーク ポリシー サーバー (NPS)](nps-top.md)します。