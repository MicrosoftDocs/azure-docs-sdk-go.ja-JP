---
title: Azure SDK for Go のサンプル (コンピューティングとネットワーク)
description: Azure SDK for Go からコンピューティング リソース (VM や仮想ネットワークなど) を操作するための厳選されたサンプルです。
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: sample
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: d570ad8598ae06633d0010245c207641161ee446
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059086"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="9b727-103">Azure SDK for Go のサンプル (コンピューティングとネットワーク)</span><span class="sxs-lookup"><span data-stu-id="9b727-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="9b727-104">次の表に、Azure SDK for Go でコンピューティング リソースと仮想ネットワーク リソースを管理する方法について説明した厳選サンプルのリンクを示します。</span><span class="sxs-lookup"><span data-stu-id="9b727-104">The following table links to selected samples that demonstrate the management of compute and virtual network resources in the Azure SDK for Go.</span></span>

<span data-ttu-id="9b727-105">Azure SDK for Go の全サンプルは、[GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples) で入手できます。</span><span class="sxs-lookup"><span data-stu-id="9b727-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="9b727-106">Name</span><span class="sxs-lookup"><span data-stu-id="9b727-106">Name</span></span> | <span data-ttu-id="9b727-107">説明</span><span class="sxs-lookup"><span data-stu-id="9b727-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="9b727-108">network/network</span><span class="sxs-lookup"><span data-stu-id="9b727-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="9b727-109">仮想ネットワーク、サブネット、ネットワーク セキュリティ グループなどのネットワーク リソースを作成、更新、削除、および照会します。</span><span class="sxs-lookup"><span data-stu-id="9b727-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="9b727-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="9b727-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="9b727-111">VM のデータ ディスクを作成、接続、切断、更新、および暗号化します。</span><span class="sxs-lookup"><span data-stu-id="9b727-111">Create, attach, detach, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="9b727-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="9b727-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="9b727-113">VM を作成、更新、非アクティブ化、および管理します。</span><span class="sxs-lookup"><span data-stu-id="9b727-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="9b727-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="9b727-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="9b727-115">VM の可用性セットおよびロード バランサーを作成します。</span><span class="sxs-lookup"><span data-stu-id="9b727-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="9b727-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="9b727-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="9b727-117">VM のマネージド サービス ID (MSI) を作成および管理します。</span><span class="sxs-lookup"><span data-stu-id="9b727-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |
