---
title: Azure SDK for Go のインストール
description: Azure SDK for Go のインストール、ベンダリング、構成の方法。
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 013a771345d96f0fa8dbece3218a01650744f70b
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059188"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="012b9-103">Azure SDK for Go のインストール</span><span class="sxs-lookup"><span data-stu-id="012b9-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="012b9-104">Azure SDK for Go へようこそ。</span><span class="sxs-lookup"><span data-stu-id="012b9-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="012b9-105">この SDK を使用すると、Go アプリケーションから Azure サービスを管理し、サービスと対話できます。</span><span class="sxs-lookup"><span data-stu-id="012b9-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="012b9-106">Azure SDK for Go を入手する</span><span class="sxs-lookup"><span data-stu-id="012b9-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="012b9-107">一部の Azure サービスには独自の Go SDK が用意されていますが、これは Azure SDK for Go のコア パッケージには含まれていません。</span><span class="sxs-lookup"><span data-stu-id="012b9-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="012b9-108">次の表に、独自の SDK が用意されているサービスと、そのパッケージ名を示します。</span><span class="sxs-lookup"><span data-stu-id="012b9-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="012b9-109">これらのパッケージは、すべてプレビュー段階にあると考えられます。</span><span class="sxs-lookup"><span data-stu-id="012b9-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="012b9-110">Service</span><span class="sxs-lookup"><span data-stu-id="012b9-110">Service</span></span> | <span data-ttu-id="012b9-111">Package</span><span class="sxs-lookup"><span data-stu-id="012b9-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="012b9-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="012b9-112">Blob Storage</span></span> | [<span data-ttu-id="012b9-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="012b9-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="012b9-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="012b9-114">File Storage</span></span> | [<span data-ttu-id="012b9-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="012b9-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="012b9-116">ストレージ キュー</span><span class="sxs-lookup"><span data-stu-id="012b9-116">Storage Queue</span></span> | [<span data-ttu-id="012b9-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="012b9-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="012b9-118">イベント ハブ</span><span class="sxs-lookup"><span data-stu-id="012b9-118">Event Hub</span></span> | [<span data-ttu-id="012b9-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="012b9-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="012b9-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="012b9-120">Service Bus</span></span> | [<span data-ttu-id="012b9-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="012b9-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="012b9-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="012b9-122">Application Insights</span></span> | [<span data-ttu-id="012b9-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="012b9-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="012b9-124">Azure SDK for Go をベンダリングする</span><span class="sxs-lookup"><span data-stu-id="012b9-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="012b9-125">Azure SDK for Go は、[dep](https://github.com/golang/dep) を使用してベンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="012b9-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="012b9-126">安定性のため、ベンダリングすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="012b9-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="012b9-127">独自のプロジェクトで `dep` を使用するには、`github.com/Azure/azure-sdk-for-go` を `Gopkg.toml` の `[[constraint]]` セクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="012b9-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="012b9-128">たとえば、バージョン `14.0.0` でベンダリングするには、次のエントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="012b9-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="012b9-129">Azure SDK for Go をプロジェクトに含める</span><span class="sxs-lookup"><span data-stu-id="012b9-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="012b9-130">Go コードから Azure サービスを使用するには、対話するサービスと必要な `autorest` モジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="012b9-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="012b9-131">提供されているモジュールの一覧については、[利用可能なサービス](https://godoc.org/github.com/Azure/azure-sdk-for-go)および [AutoRest パッケージ](https://godoc.org/github.com/Azure/go-autorest) の GoDoc を参照してください。</span><span class="sxs-lookup"><span data-stu-id="012b9-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="012b9-132">必要となる `go-autorest` の最も一般的なパッケージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="012b9-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="012b9-133">Package</span><span class="sxs-lookup"><span data-stu-id="012b9-133">Package</span></span> | <span data-ttu-id="012b9-134">説明</span><span class="sxs-lookup"><span data-stu-id="012b9-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="012b9-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="012b9-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="012b9-136">サービス クライアント認証を処理するためのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="012b9-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="012b9-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="012b9-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="012b9-138">Azure サービスと対話するための定数</span><span class="sxs-lookup"><span data-stu-id="012b9-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="012b9-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="012b9-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="012b9-140">Azure サービスにアクセスするための認証メカニズム</span><span class="sxs-lookup"><span data-stu-id="012b9-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="012b9-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="012b9-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="012b9-142">Azure SDK のデータ構造を操作するための型アサーション ヘルパー</span><span class="sxs-lookup"><span data-stu-id="012b9-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="012b9-143">Go パッケージと Azure サービスは、個別にバージョン管理されます。</span><span class="sxs-lookup"><span data-stu-id="012b9-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="012b9-144">サービスのバージョンは、`services` モジュールの下の、モジュールのインポート パスに含まれます。</span><span class="sxs-lookup"><span data-stu-id="012b9-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="012b9-145">モジュールの完全なパスは、サービス名の後に `YYYY-MM-DD` 形式のバージョンが続き、その後にもう一度サービス名が続きます。</span><span class="sxs-lookup"><span data-stu-id="012b9-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="012b9-146">たとえば、Compute サービスの `2017-03-30` バージョンをインポートするには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="012b9-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="012b9-147">開発の開始時には最新バージョンのサービスを使用して、一貫性を保つことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="012b9-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="012b9-148">バージョン間で Go SDK の更新プログラムが公開されない場合でも、その期間中にコードの破損につながりかねないようなサービス要件の変更が発生することがあるためです。</span><span class="sxs-lookup"><span data-stu-id="012b9-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="012b9-149">サービスの一括スナップショットが必要な場合は、単一のプロファイル バージョンを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="012b9-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="012b9-150">現時点では、ロックされたプロファイルはバージョン `2017-03-09` だけです。このバージョンには、サービスの最新の機能が含まれていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="012b9-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="012b9-151">プロファイルは `profiles` モジュールの下にあり、バージョンは `YYYY-MM-DD` 形式です。</span><span class="sxs-lookup"><span data-stu-id="012b9-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="012b9-152">サービスは、プロファイル バージョンの下にグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="012b9-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="012b9-153">たとえば、`2017-03-09` プロファイルから Azure リソース管理モジュールをインポートするには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="012b9-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="012b9-154">`preview`、`latest` の各プロファイルも用意されています。</span><span class="sxs-lookup"><span data-stu-id="012b9-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="012b9-155">これらは使用しないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="012b9-155">Using them is not recommended.</span></span> <span data-ttu-id="012b9-156">これらのプロファイルはローリング バージョンであり、サービスの動作が変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="012b9-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="012b9-157">次の手順</span><span class="sxs-lookup"><span data-stu-id="012b9-157">Next steps</span></span>

<span data-ttu-id="012b9-158">Azure SDK for Go を使い始めるときには、クイック スタートをお試しください。</span><span class="sxs-lookup"><span data-stu-id="012b9-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="012b9-159">テンプレートから仮想マシンをデプロイする</span><span class="sxs-lookup"><span data-stu-id="012b9-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="012b9-160">Azure Blob SDK for Go を使用して Azure Blob Storage にオブジェクトを転送する</span><span class="sxs-lookup"><span data-stu-id="012b9-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="012b9-161">Azure Database for PostgreSQL に接続する</span><span class="sxs-lookup"><span data-stu-id="012b9-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="012b9-162">Go SDK で他のサービスをすぐに使用する場合は、用意されているサンプル コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="012b9-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="012b9-163">Azure サービスによる認証</span><span class="sxs-lookup"><span data-stu-id="012b9-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="012b9-164">SSH 認証を使用した新しい仮想マシンのデプロイ</span><span class="sxs-lookup"><span data-stu-id="012b9-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="012b9-165">Azure Container Instances へのコンテナー イメージのデプロイ</span><span class="sxs-lookup"><span data-stu-id="012b9-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="012b9-166">Azure Kubernetes Service でのクラスターの作成</span><span class="sxs-lookup"><span data-stu-id="012b9-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="012b9-167">Azure Storage サービスの操作</span><span class="sxs-lookup"><span data-stu-id="012b9-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="012b9-168">Azure SDK for Go のすべてのサンプル</span><span class="sxs-lookup"><span data-stu-id="012b9-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
