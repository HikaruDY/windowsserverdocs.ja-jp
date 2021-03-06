---
title: 使用中の論理プロセッサの数は、サポートされている最大を超えていない必要があります。
description: このベストプラクティスアナライザー規則によって報告された問題を解決するための手順を示します。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 380daf333c041c8702228a60c26ab6e76e4cf3e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393398"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>使用中の論理プロセッサの数は、サポートされている最大を超えていない必要があります。

>適用先:Windows Server 2016

ベスト プラクティスとスキャンの詳細については、「 [ベスト プラクティス アナライザー](https://go.microsoft.com/fwlink/?LinkId=122786)」をご覧ください。  
  
|プロパティ|詳細|  
|-|-|  
|**オペレーティング システム**|Windows Server 2016|  
|**製品/機能**|Hyper-V|  
|**順**|Error|  
|**カテゴリ**|ポリシー|  
  
次のセクションでは、斜体は、この問題のためのベスト プラクティス アナライザー ツールで表示されるテキストを示します。  
  
## <a name="issue"></a>問題  
  
*サーバーの構成に使用されている論理プロセッサが多すぎます。*  
  
## <a name="impact"></a>影響  
  
*Microsoft では、このコンピューターでの Hyper-v の実行をサポートしていません。*  
  
## <a name="resolution"></a>解決方法  
  
*このマシンからプロセッサをいくつか削除するか、msconfig を使用して使用可能なプロセッサの数を制限してください。*  
  
Msconfig を使用して、次の手順を参照してください。 プロセッサを削除する方法については、コンピューターに付属の指示を参照またはハードウェアの製造元に問い合わせてください。 HYPER-V の最大のサポートされる構成の詳細については、「 [Windows Server 2016 での HYPER-V のスケーラビリティの計画](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)します。  
  
### <a name="to-limit-the-number-of-available-processors"></a>使用可能なプロセッサ数を制限するには  
  
1.  開くと、システム構成アプリケーション (Msconfig.exe)。 これを行うには、次のようにクリックします。 **開始**, 、型 **msconfig**, 、を右クリックし、 **システム構成** デスクトップ アプリをクリック **管理者として実行**します。  
  
2.  **ブート** ] タブ、[ **詳細オプション**します。  
  
3.  選択 **プロセッサの数** し、一覧で番号を選択します。 **[OK]** をクリックします。  
  
4.  プロセッサの新しい番号を使用して実行するコンピューターを再起動します。  
  


