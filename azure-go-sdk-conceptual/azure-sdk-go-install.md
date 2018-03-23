---
title: Azure SDK for Go のインストール
description: Azure SDK for Go のインストール、ベンダリング、構成の方法。
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 580daf4f2e91eabf97e3acd21bda183c559b57da
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2018
---
# <a name="installing-the-azure-sdk-for-go"></a><span data-ttu-id="d6584-103">Azure SDK for Go のインストール</span><span class="sxs-lookup"><span data-stu-id="d6584-103">Installing the Azure SDK for Go</span></span>

<span data-ttu-id="d6584-104">Azure SDK for Go へようこそ。</span><span class="sxs-lookup"><span data-stu-id="d6584-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="d6584-105">この SDK を使用すると、Go アプリケーションから Azure サービスを管理し、サービスと対話できます。</span><span class="sxs-lookup"><span data-stu-id="d6584-105">This SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="d6584-106">Azure SDK for Go を入手する</span><span class="sxs-lookup"><span data-stu-id="d6584-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="d6584-107">Azure Storage Blob を操作するには、別の SDK が必要です。</span><span class="sxs-lookup"><span data-stu-id="d6584-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a><span data-ttu-id="d6584-108">Azure SDK for Go をベンダリングする</span><span class="sxs-lookup"><span data-stu-id="d6584-108">Vendoring the Azure SDK for Go</span></span>

<span data-ttu-id="d6584-109">Azure SDK for Go は、[dep](https://github.com/golang/dep) を使用してベンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="d6584-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="d6584-110">安定性のため、ベンダリングすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d6584-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="d6584-111">`dep` サポートを使用するには、`Gopkg.toml` の `[[constraint]]` セクションに `github.com/Azure/azure-sdk-for-go` を追加します。</span><span class="sxs-lookup"><span data-stu-id="d6584-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="d6584-112">たとえば、バージョン `14.0.0` でベンダリングするには、次のエントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="d6584-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="d6584-113">Azure SDK for Go をプロジェクトに含める</span><span class="sxs-lookup"><span data-stu-id="d6584-113">Including the Azure SDK for Go in your project</span></span>

<span data-ttu-id="d6584-114">Go コードから Azure サービスを使用するには、対話するサービスと必要な `autorest` モジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="d6584-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="d6584-115">提供されているモジュールの一覧については、[利用可能なサービス](https://godoc.org/github.com/Azure/azure-sdk-for-go)および [AutoRest パッケージ](https://godoc.org/github.com/Azure/go-autorest) の GoDoc を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d6584-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="d6584-116">必要となる `go-autorest` の最も一般的なパッケージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d6584-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="d6584-117">パッケージ</span><span class="sxs-lookup"><span data-stu-id="d6584-117">Package</span></span> | <span data-ttu-id="d6584-118">[説明]</span><span class="sxs-lookup"><span data-stu-id="d6584-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="d6584-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="d6584-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="d6584-120">サービス クライアント認証を処理するためのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="d6584-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="d6584-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="d6584-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="d6584-122">Azure サービスと対話するための定数</span><span class="sxs-lookup"><span data-stu-id="d6584-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="d6584-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="d6584-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="d6584-124">Azure サービスにアクセスするための認証メカニズム</span><span class="sxs-lookup"><span data-stu-id="d6584-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="d6584-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="d6584-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="d6584-126">Azure SDK のデータ構造を操作するための型アサーション ヘルパー</span><span class="sxs-lookup"><span data-stu-id="d6584-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="d6584-127">Azure サービス用のモジュールは、SDK API とは別にバージョン管理されています。</span><span class="sxs-lookup"><span data-stu-id="d6584-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="d6584-128">これらのバージョンは、モジュールのインポート パスに含まれ、_サービス バージョン_または_プロファイル_のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="d6584-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="d6584-129">現時点では、特定のサービス バージョンを開発とリリースの両方に使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d6584-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="d6584-130">サービスは、`services` モジュールの下にあります。</span><span class="sxs-lookup"><span data-stu-id="d6584-130">Services are located under the `services` module.</span></span> <span data-ttu-id="d6584-131">インポートの完全なパスは、サービス名の後に `YYYY-MM-DD` 形式のバージョンが続き、その後にもう一度サービス名が続きます。</span><span class="sxs-lookup"><span data-stu-id="d6584-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="d6584-132">たとえば、Compute サービスの `2017-03-30` バージョンを含めるには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="d6584-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="d6584-133">他のバージョンを使用する理由がない限り、現時点ではサービスの最新バージョンを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d6584-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="d6584-134">サービスの一括スナップショットが必要な場合は、単一のプロファイル バージョンを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="d6584-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="d6584-135">現時点では、ロックされたプロファイルはバージョン `2017-03-30` だけです。このバージョンには、サービスの最新の機能が含まれていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d6584-135">Right now, the only locked profile is version `2017-03-30`, which may not have the latest features of services.</span></span> <span data-ttu-id="d6584-136">プロファイルは `profiles` モジュールの下にあり、バージョンは `YYYY-MM-DD` 形式です。</span><span class="sxs-lookup"><span data-stu-id="d6584-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="d6584-137">サービスは、プロファイル バージョンの下にグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="d6584-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="d6584-138">たとえば、`2017-03-09` プロファイルから Azure リソース管理モジュールをインポートするには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="d6584-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="d6584-139">`preview`、`latest` の各プロファイルも用意されています。</span><span class="sxs-lookup"><span data-stu-id="d6584-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="d6584-140">これらは使用しないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d6584-140">Using them is not recommended.</span></span> <span data-ttu-id="d6584-141">これらのプロファイルはローリング バージョンであり、サービスの動作が変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d6584-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6584-142">次の手順</span><span class="sxs-lookup"><span data-stu-id="d6584-142">Next steps</span></span>

<span data-ttu-id="d6584-143">Azure SDK for Go を使い始めるときには、クイック スタートをお試しください。</span><span class="sxs-lookup"><span data-stu-id="d6584-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="d6584-144">テンプレートから仮想マシンをデプロイする</span><span class="sxs-lookup"><span data-stu-id="d6584-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="d6584-145">Azure Blob SDK for Go を使用して Azure Blob Storage にオブジェクトを転送する</span><span class="sxs-lookup"><span data-stu-id="d6584-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="d6584-146">Azure Database for PostgreSQL に接続する</span><span class="sxs-lookup"><span data-stu-id="d6584-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="d6584-147">Go SDK で他のサービスをすぐに使用する場合は、用意されているサンプル コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d6584-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="d6584-148">Azure サービスによる認証</span><span class="sxs-lookup"><span data-stu-id="d6584-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="d6584-149">SSH 認証を使用した新しい仮想マシンのデプロイ</span><span class="sxs-lookup"><span data-stu-id="d6584-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="d6584-150">Azure Container Instances へのコンテナー イメージのデプロイ</span><span class="sxs-lookup"><span data-stu-id="d6584-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="d6584-151">Azure Kubernetes サービスでのクラスターの作成</span><span class="sxs-lookup"><span data-stu-id="d6584-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="d6584-152">Azure Storage サービスの操作</span><span class="sxs-lookup"><span data-stu-id="d6584-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="d6584-153">Azure SDK for Go のすべてのサンプル</span><span class="sxs-lookup"><span data-stu-id="d6584-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
