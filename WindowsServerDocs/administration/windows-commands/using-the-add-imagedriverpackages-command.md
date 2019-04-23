---
title: 追加 ImageDriverPackages コマンドを使用してください。
description: 'Windows コマンド」のトピック * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55b26acf9c4006db3d64e27be8a10e158876ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817363"
---
# <a name="using-the-add-imagedriverpackages-command"></a>追加 ImageDriverPackages コマンドを使用してください。

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

ブート イメージにドライバー ストアからドライバー パッケージを追加します。 イメージのバージョンは、Windows 7 または Windows Server 2008 R2 をする必要がありますまたはそれ以降。
このコマンドを使用する方法の例については、次を参照してください。[例](#BKMK_examples)します。
## <a name="syntax"></a>構文
```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
[/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
## <a name="parameters"></a>パラメーター
|パラメーター|説明|
|-------|--------|
|/Server:<Server name>|サーバーの名前を指定します。 これには、NetBIOS 名または FQDN を指定できます。 サーバー名が指定されていない場合は、ローカル サーバーが使用されます。|
メディア:<Image name>|ドライバーを追加するイメージの名前を指定します。|
mediatype:Boot|ドライバーを追加する画像の種類を指定します。 ドライバー パッケージは、ブート イメージにのみ追加できます。|
|/アーキテクチャ: {x86 & #124; ia64 & #124; x64}|ブート イメージのアーキテクチャを指定します。 さまざまなアーキテクチャでブート イメージで同じイメージの名前を指定することも可能であるために、確実に適切なイメージが使用されるアーキテクチャを指定してください。|
|/Filename:<File name>|ファイル名を指定します。 イメージは、名前によって一意に識別できない、ファイル名を指定してください。|
|/Filtertype:<Filter type>|検索するドライバー パッケージの属性を指定します。 1 つのコマンドでは、複数の属性を指定できます。 指定する必要があります **に** と **/value** このオプションを使用します。<br /><br /><Filter type> 次のいずれかを指定できます。<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/演算子: {等号と #124 文字です。NotEqual & #124 文字です。GreaterOrEqual & #124 文字です。LessOrEqual & #124 文字です。含まれています|属性と値の間のリレーションシップです。 だけを指定できます **Contains** 文字列の属性を持つ。 だけを指定できます **GreaterOrEqual** と **LessOrEqual** 日、バージョン属性を持つ。|
|/値:<Value>|指定した基準を検索する値を指定 <attribute>します。 1 つの複数の値を指定する **/Filtertype**します。 各フィルターに指定できる属性のアウトライン以下の一覧です。 これらの属性の詳細については、次を参照してください。[ドライバーとパッケージの属性](https://go.microsoft.com/fwlink/?LinkId=166895)(https://go.microsoft.com/fwlink/?LinkId=166895)します。<br /><br />-PackageId - は、有効な GUID を指定します。 例: {4d36e972-e325-11ce-bfc1-08002be10318} します。<br />-パッケージ名を指定する任意の文字列値。<br />-PackageEnabled - 指定 **はい** または **いいえ**します。<br />-Packagedateadded - は、次の形式で日付を指定します。YYYY DD MM<br />-Packageinffilename を指定する任意の文字列値。<br />-PackageClass - は、有効なクラス名またはクラス GUID を指定します。 例:**ディスク ドライブ**、 **Net**、または {4d36e972-e325-11ce-bfc1-08002be10318} します。<br />-Packageprovider を指定する任意の文字列値。<br />-PackageArchitecture - 指定 **x86**,  、**x64**, 、または **ia64**.<b />-PackageLocale - は、有効な言語識別子を指定します。 例: **EN-US** または **ES-ES**します。<br />-PackageSigned - 指定 **はい** または **いいえ**します。<br />-Packagedatepublished - は、次の形式で日付を指定します。YYYY DD MM<br />-Packageversion - は、次の形式でバージョンを指定: します a.b.x.y. 例:6.1.0.0<br />-Driverdescription を指定する任意の文字列値。<br />-Drivermanufacturer を指定する任意の文字列値。<br />-DriverHardwareId - は、任意の文字列値を指定します。<br />--Drivercompatibleid は、任意の文字列値を指定します。<br />-DriverExcludeId - は、任意の文字列値を指定します。<br />-DriverGroupId - は、有効な GUID を指定します。 例: {4d36e972-e325-11ce-bfc1-08002be10318} します。<br />-Drivergroupname を指定する任意の文字列値。|
## <a name="BKMK_examples"></a>例
ブート イメージにドライバー パッケージを追加するには、次のいずれかを入力します。
```
wdsutil /add-ImageDriverPackagemedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```
```
wdsutil /verbose /add-ImageDriverPackagemedia: "WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
```
wdsutil /verbose /add-ImageDriverPackagemedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```
#### <a name="additional-references"></a>その他の参照
[コマンドライン構文のポイント](command-line-syntax-key.md)
[追加 ImageDriverPackage コマンドを使用して](using-the-add-imagedriverpackage-command.md)
[追加 AllDriverPackages サブコマンドを使用します。](using-the-add-alldriverpackages-subcommand.md)