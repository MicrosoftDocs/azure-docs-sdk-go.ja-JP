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
ms.openlocfilehash: 4837572a50ae934e71bfe49d916c01e131bb6d83
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="8ccde-103">Azure SDK for Go のサンプル (コンピューティングとネットワーク)</span><span class="sxs-lookup"><span data-stu-id="8ccde-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="8ccde-104">次の表に、Azure で VM、仮想ネットワーク、サブネットを管理するために使用できる、Go ソース コードの厳選サンプルのリンクを示します。</span><span class="sxs-lookup"><span data-stu-id="8ccde-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="8ccde-105">Azure SDK for Go の全サンプルは、[GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples) で入手できます。</span><span class="sxs-lookup"><span data-stu-id="8ccde-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="8ccde-106">Name</span><span class="sxs-lookup"><span data-stu-id="8ccde-106">Name</span></span> | <span data-ttu-id="8ccde-107">[説明]</span><span class="sxs-lookup"><span data-stu-id="8ccde-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="8ccde-108">network/network</span><span class="sxs-lookup"><span data-stu-id="8ccde-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="8ccde-109">仮想ネットワーク、サブネット、ネットワーク セキュリティ グループなどのネットワーク リソースを作成、更新、削除、および照会します。</span><span class="sxs-lookup"><span data-stu-id="8ccde-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="8ccde-110">compute/loadbalancer</span><span class="sxs-lookup"><span data-stu-id="8ccde-110">compute/loadbalancer</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/loadbalancer.go) | <span data-ttu-id="8ccde-111">可用性グループを作成および照会し、ロード バランサーを使用して VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="8ccde-111">Create and query availability groups, and create VMs with a load balancer.</span></span> |
| [<span data-ttu-id="8ccde-112">compute/compute</span><span class="sxs-lookup"><span data-stu-id="8ccde-112">compute/compute</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/compute.go) | <span data-ttu-id="8ccde-113">VM の作成、削除、更新、および電源管理を行います。</span><span class="sxs-lookup"><span data-stu-id="8ccde-113">Create, delete, update, and power-manage VMs.</span></span> <span data-ttu-id="8ccde-114">VM のデータ ディスクの操作および OS ディスクの管理を行います。</span><span class="sxs-lookup"><span data-stu-id="8ccde-114">Work with data disks and managing the OS disk of the VM.</span></span> |
