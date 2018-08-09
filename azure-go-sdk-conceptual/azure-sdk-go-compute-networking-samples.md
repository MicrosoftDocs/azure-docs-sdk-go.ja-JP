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
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="39129-103">Azure SDK for Go のサンプル (コンピューティングとネットワーク)</span><span class="sxs-lookup"><span data-stu-id="39129-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="39129-104">次の表に、Azure で VM、仮想ネットワーク、サブネットを管理するために使用できる、Go ソース コードの厳選サンプルのリンクを示します。</span><span class="sxs-lookup"><span data-stu-id="39129-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="39129-105">Azure SDK for Go の全サンプルは、[GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples) で入手できます。</span><span class="sxs-lookup"><span data-stu-id="39129-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="39129-106">Name</span><span class="sxs-lookup"><span data-stu-id="39129-106">Name</span></span> | <span data-ttu-id="39129-107">説明</span><span class="sxs-lookup"><span data-stu-id="39129-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="39129-108">network/network</span><span class="sxs-lookup"><span data-stu-id="39129-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="39129-109">仮想ネットワーク、サブネット、ネットワーク セキュリティ グループなどのネットワーク リソースを作成、更新、削除、および照会します。</span><span class="sxs-lookup"><span data-stu-id="39129-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="39129-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="39129-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="39129-111">VM のデータ ディスクを作成、接続、切断、更新、および暗号化します。</span><span class="sxs-lookup"><span data-stu-id="39129-111">Create, attach, detatch, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="39129-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="39129-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="39129-113">VM を作成、更新、非アクティブ化、および管理します。</span><span class="sxs-lookup"><span data-stu-id="39129-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="39129-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="39129-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="39129-115">VM の可用性セットおよびロード バランサーを作成します。</span><span class="sxs-lookup"><span data-stu-id="39129-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="39129-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="39129-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="39129-117">VM のマネージド サービス ID (MSI) を作成および管理します。</span><span class="sxs-lookup"><span data-stu-id="39129-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |
