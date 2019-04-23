---
title: QoS ポリシーのしくみ
description: このトピックでは、グループ ポリシーを使用して、特定のアプリケーションと Windows Server 2016 でのサービスのネットワーク トラフィックの帯域幅の優先順位を設定することができるサービスの品質 (QoS) ポリシーの概要を示します。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1073308b5939e648fdcc2006acdce76ecf0331c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="how-qos-policy-works"></a>QoS ポリシーのしくみ

>適用対象: Windows Server (半期チャネル)、Windows Server 2016

起動時または更新されたユーザーまたは QoS のコンピューターの構成のグループ ポリシー設定を取得すると、次の処理が発生します。

1. グループ ポリシー エンジンは、Active Directory からユーザーまたはコンピューターの構成のグループ ポリシー設定を取得します。

2. QoS ポリシーの変更があった QoS クライアント側拡張のグループ ポリシー エンジンに通知します。

3. QoS のクライアント側拡張機能は、QoS 検査モジュールを QoS ポリシーのイベント通知を送信します。

4. QoS 検査モジュールは、ユーザーまたはコンピューターの QoS ポリシーを取得し、それらを格納します。

ときに、新しいトランスポート層エンドポイント \ (接続の TCP または UDP traffic\) 作成されると、次の処理が行われます。

1. TCP/IP スタックのトランスポート層コンポーネントは、QoS 検査モジュールを通知します。

2. QoS 検査モジュールは、トランスポート層エンドポイントに格納される QoS ポリシーのパラメーターを比較します。

3. マッチが見つかった場合、QoS 検査モジュールはフロー、DSCP 値と一致する QoS ポリシーの設定を調整するトラフィックを含むデータ構造を作成する Pacer.sys を接続します。 トランスポート層のエンドポイントのパラメーターに一致する複数の QoS ポリシーがある場合は、最も固有の QoS ポリシーが使用されます。

4. Pacer.sys はフローに保管され、QoS 検査モジュールへのフローに対応するフロー数を返します。

5. QoS 検査モジュールは、トランスポート層にフローの数を返します。

6. トランスポート層は、トランスポート層エンドポイントとフロー番号を格納します。

トランスポート層エンドポイントに対応するパケットがフロー数でマークされている場合は、送信次の処理が行われます。

1. トランスポート層に、内部的にフロー番号を持つパケットをマークします。

2. ネットワーク レイヤーは、パケットのフロー番号に対応する DSCP 値を Pacer.sys を照会します。

3. Pacer.sys では、ネットワーク層に DSCP 値を返します。

4. ネットワーク レイヤーでは、Pacer.sys によって指定された DSCP 値に IPv4 TOS フィールドまたは IPv6 Traffic Class フィールドを変更して、IPv4 パケットの場合は、最終的な IPv4 ヘッダーのチェックサムを計算します。

5. ネットワーク レイヤーは、フレームのレイヤーにパケットを渡します。

6. パケット フローの数とマークされているため、フレームのレイヤーに渡し、パケット NDIS を通じて Pacer.sys 6.x します。

7. Pacer.sys は、フローのパケット数、パケットを調整する必要がある場合、およびその場合を判断する、パケットを送信するようにスケジュールします。

8. Pacer.sys パケットに渡し、か、すぐに \ (トラフィック throttling\ がない場合) またはスケジュールどおりに \ (トラフィック throttling\ がある場合) NDIS に適切なネットワーク アダプタ上で転送するための 6.x します。

ポリシー ベースの QoS のこれらのプロセスでは、次の利点を提供します。

- パケット単位ではなく-トランスポート層のエンドポイントは、QoS ポリシーを適用するかどうかを決定するトラフィックの検査が行われます。

- QoS ポリシーと一致しないトラフィックのパフォーマンスに影響はありません。

- アプリケーションは、DSCP に基づいた差別化されたサービスを利用したり、トラフィックの調整を変更する必要はありません。

- QoS ポリシーは、IPsec で保護されたトラフィックに適用できます。

このガイドの次のトピックを参照してください。[QoS ポリシーのアーキテクチャ](qos-policy-architecture.md)します。

このガイドで最初のトピックを参照してください。[サービスの品質 (QoS) ポリシー](qos-policy-top.md)します。