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
ms.openlocfilehash: 8423b3fedd89e57662bf96f777a5b30926914da9
ms.sourcegitcommit: b81b17cbb934399c195bfdcb87137aee935f5234
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34755517"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="b9803-103">Azure SDK for Go のインストール</span><span class="sxs-lookup"><span data-stu-id="b9803-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="b9803-104">Azure SDK for Go へようこそ。</span><span class="sxs-lookup"><span data-stu-id="b9803-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="b9803-105">この SDK を使用すると、Go アプリケーションから Azure サービスを管理し、サービスと対話できます。</span><span class="sxs-lookup"><span data-stu-id="b9803-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="b9803-106">Azure SDK for Go を入手する</span><span class="sxs-lookup"><span data-stu-id="b9803-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="b9803-107">一部の Azure サービスには独自の Go SDK が用意されていますが、これは Azure SDK for Go のコア パッケージには含まれていません。</span><span class="sxs-lookup"><span data-stu-id="b9803-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="b9803-108">次の表に、独自の SDK が用意されているサービスと、そのパッケージ名を示します。</span><span class="sxs-lookup"><span data-stu-id="b9803-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="b9803-109">これらのパッケージは、すべてプレビュー段階にあると考えられます。</span><span class="sxs-lookup"><span data-stu-id="b9803-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="b9803-110">サービス</span><span class="sxs-lookup"><span data-stu-id="b9803-110">Service</span></span> | <span data-ttu-id="b9803-111">パッケージ</span><span class="sxs-lookup"><span data-stu-id="b9803-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="b9803-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="b9803-112">Blob Storage</span></span> | [<span data-ttu-id="b9803-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="b9803-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="b9803-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="b9803-114">File Storage</span></span> | [<span data-ttu-id="b9803-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="b9803-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="b9803-116">イベント ハブ</span><span class="sxs-lookup"><span data-stu-id="b9803-116">Event Hub</span></span> | [<span data-ttu-id="b9803-117">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="b9803-117">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="b9803-118">Application Insights</span><span class="sxs-lookup"><span data-stu-id="b9803-118">Application Insights</span></span> | [<span data-ttu-id="b9803-119">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="b9803-119">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="b9803-120">Azure SDK for Go をベンダリングする</span><span class="sxs-lookup"><span data-stu-id="b9803-120">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="b9803-121">Azure SDK for Go は、[dep](https://github.com/golang/dep) を使用してベンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="b9803-121">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="b9803-122">安定性のため、ベンダリングすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b9803-122">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="b9803-123">`dep` サポートを使用するには、`Gopkg.toml` の `[[constraint]]` セクションに `github.com/Azure/azure-sdk-for-go` を追加します。</span><span class="sxs-lookup"><span data-stu-id="b9803-123">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="b9803-124">たとえば、バージョン `14.0.0` でベンダリングするには、次のエントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="b9803-124">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="b9803-125">Azure SDK for Go をプロジェクトに含める</span><span class="sxs-lookup"><span data-stu-id="b9803-125">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="b9803-126">Go コードから Azure サービスを使用するには、対話するサービスと必要な `autorest` モジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="b9803-126">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="b9803-127">提供されているモジュールの一覧については、[利用可能なサービス](https://godoc.org/github.com/Azure/azure-sdk-for-go)および [AutoRest パッケージ](https://godoc.org/github.com/Azure/go-autorest) の GoDoc を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b9803-127">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="b9803-128">必要となる `go-autorest` の最も一般的なパッケージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="b9803-128">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="b9803-129">パッケージ</span><span class="sxs-lookup"><span data-stu-id="b9803-129">Package</span></span> | <span data-ttu-id="b9803-130">説明</span><span class="sxs-lookup"><span data-stu-id="b9803-130">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="b9803-131">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="b9803-131">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="b9803-132">サービス クライアント認証を処理するためのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="b9803-132">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="b9803-133">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="b9803-133">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="b9803-134">Azure サービスと対話するための定数</span><span class="sxs-lookup"><span data-stu-id="b9803-134">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="b9803-135">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="b9803-135">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="b9803-136">Azure サービスにアクセスするための認証メカニズム</span><span class="sxs-lookup"><span data-stu-id="b9803-136">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="b9803-137">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="b9803-137">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="b9803-138">Azure SDK のデータ構造を操作するための型アサーション ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9803-138">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="b9803-139">Azure サービス用のモジュールは、SDK API とは別にバージョン管理されています。</span><span class="sxs-lookup"><span data-stu-id="b9803-139">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="b9803-140">これらのバージョンは、モジュールのインポート パスに含まれ、_サービス バージョン_または_プロファイル_のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="b9803-140">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="b9803-141">現時点では、特定のサービス バージョンを開発とリリースの両方に使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b9803-141">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="b9803-142">サービスは、`services` モジュールの下にあります。</span><span class="sxs-lookup"><span data-stu-id="b9803-142">Services are located under the `services` module.</span></span> <span data-ttu-id="b9803-143">インポートの完全なパスは、サービス名の後に `YYYY-MM-DD` 形式のバージョンが続き、その後にもう一度サービス名が続きます。</span><span class="sxs-lookup"><span data-stu-id="b9803-143">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="b9803-144">たとえば、Compute サービスの `2017-03-30` バージョンを含めるには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="b9803-144">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="b9803-145">他のバージョンを使用する理由がない限り、現時点ではサービスの最新バージョンを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b9803-145">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="b9803-146">サービスの一括スナップショットが必要な場合は、単一のプロファイル バージョンを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="b9803-146">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="b9803-147">現時点では、ロックされたプロファイルはバージョン `2017-03-09` だけです。このバージョンには、サービスの最新の機能が含まれていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b9803-147">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="b9803-148">プロファイルは `profiles` モジュールの下にあり、バージョンは `YYYY-MM-DD` 形式です。</span><span class="sxs-lookup"><span data-stu-id="b9803-148">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="b9803-149">サービスは、プロファイル バージョンの下にグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="b9803-149">Services are grouped under their profile version.</span></span> <span data-ttu-id="b9803-150">たとえば、`2017-03-09` プロファイルから Azure リソース管理モジュールをインポートするには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="b9803-150">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="b9803-151">`preview`、`latest` の各プロファイルも用意されています。</span><span class="sxs-lookup"><span data-stu-id="b9803-151">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="b9803-152">これらは使用しないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b9803-152">Using them is not recommended.</span></span> <span data-ttu-id="b9803-153">これらのプロファイルはローリング バージョンであり、サービスの動作が変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b9803-153">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9803-154">次の手順</span><span class="sxs-lookup"><span data-stu-id="b9803-154">Next steps</span></span>

<span data-ttu-id="b9803-155">Azure SDK for Go を使い始めるときには、クイック スタートをお試しください。</span><span class="sxs-lookup"><span data-stu-id="b9803-155">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="b9803-156">テンプレートから仮想マシンをデプロイする</span><span class="sxs-lookup"><span data-stu-id="b9803-156">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="b9803-157">Azure Blob SDK for Go を使用して Azure Blob Storage にオブジェクトを転送する</span><span class="sxs-lookup"><span data-stu-id="b9803-157">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="b9803-158">Azure Database for PostgreSQL に接続する</span><span class="sxs-lookup"><span data-stu-id="b9803-158">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="b9803-159">Go SDK で他のサービスをすぐに使用する場合は、用意されているサンプル コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b9803-159">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="b9803-160">Azure サービスによる認証</span><span class="sxs-lookup"><span data-stu-id="b9803-160">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="b9803-161">SSH 認証を使用した新しい仮想マシンのデプロイ</span><span class="sxs-lookup"><span data-stu-id="b9803-161">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="b9803-162">Azure Container Instances へのコンテナー イメージのデプロイ</span><span class="sxs-lookup"><span data-stu-id="b9803-162">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* <span data-ttu-id="b9803-163">
  [Azure Kubernetes Service でのクラスターの作成](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)</span><span class="sxs-lookup"><span data-stu-id="b9803-163">[Create a cluster in Azure Kubernetes Service](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)</span></span>
* [<span data-ttu-id="b9803-164">Azure Storage サービスの操作</span><span class="sxs-lookup"><span data-stu-id="b9803-164">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="b9803-165">Azure SDK for Go のすべてのサンプル</span><span class="sxs-lookup"><span data-stu-id="b9803-165">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
