---
title: Azure SDK for Go のサンプル (コンピューティングとネットワーク)
description: Azure SDK for Go からコンピューティング リソース (VM や仮想ネットワークなど) を操作するための厳選されたサンプルです。
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/21/2018
ms.topic: sample
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 3b31716ee42c638bab4a6dd99b9eb0d7c07e51a4
ms.sourcegitcommit: 0f581979216f7c9d4913681a6d9f6fe09af26e43
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2018
ms.locfileid: "39475791"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a>Azure SDK for Go のサンプル (コンピューティングとネットワーク)

次の表に、Azure で VM、仮想ネットワーク、サブネットを管理するために使用できる、Go ソース コードの厳選サンプルのリンクを示します。 

Azure SDK for Go の全サンプルは、[GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples) で入手できます。

| Name | 説明 |
|------|-------------|
| [network/network](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | 仮想ネットワーク、サブネット、ネットワーク セキュリティ グループなどのネットワーク リソースを作成、更新、削除、および照会します。 |
| [compute/vm_disk](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | VM のデータ ディスクを作成、接続、切断、更新、および暗号化します。 |
| [compute/vm](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | VM を作成、更新、非アクティブ化、および管理します。 |
| [compute/vm_with_availabilityset](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | VM の可用性セットおよびロード バランサーを作成します。 |
| [compute/vm_with_identity](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | VM のマネージド サービス ID (MSI) を作成および管理します。 |
