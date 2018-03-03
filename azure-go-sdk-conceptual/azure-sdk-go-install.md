---
title: "Azure SDK for Go のインストール"
description: "Azure SDK for Go のインストール、ベンダリング、構成の方法。"
keywords: azure, sdk, go, golang, azure sdk
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 7fc0a3ff71b0b06f616ae43cff311352fe873345
ms.sourcegitcommit: 890f5f01a70e7e376e6bb98a2030afbfc016f538
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/01/2018
---
# <a name="installing-the-azure-sdk-for-go"></a><span data-ttu-id="a54a2-104">Azure SDK for Go のインストール</span><span class="sxs-lookup"><span data-stu-id="a54a2-104">Installing the Azure SDK for Go</span></span>

<span data-ttu-id="a54a2-105">Azure SDK for Go へようこそ。</span><span class="sxs-lookup"><span data-stu-id="a54a2-105">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="a54a2-106">この SDK を使用すると、Go アプリケーションから Azure サービスを管理し、サービスと対話できます。</span><span class="sxs-lookup"><span data-stu-id="a54a2-106">This SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="a54a2-107">Azure SDK for Go を入手する</span><span class="sxs-lookup"><span data-stu-id="a54a2-107">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="a54a2-108">Azure Storage Blob を操作するには、別の SDK が必要です。</span><span class="sxs-lookup"><span data-stu-id="a54a2-108">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a><span data-ttu-id="a54a2-109">Azure SDK for Go をベンダリングする</span><span class="sxs-lookup"><span data-stu-id="a54a2-109">Vendoring the Azure SDK for Go</span></span>

<span data-ttu-id="a54a2-110">Azure SDK for Go は、[dep](https://github.com/golang/dep) を使用してベンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="a54a2-110">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="a54a2-111">安定性のため、ベンダリングすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a54a2-111">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="a54a2-112">`dep` サポートを使用するには、`Gopkg.toml` の `[[constraint]]` セクションに `github.com/Azure/azure-sdk-for-go` を追加します。</span><span class="sxs-lookup"><span data-stu-id="a54a2-112">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="a54a2-113">たとえば、バージョン `14.0.0` でベンダリングするには、次のエントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="a54a2-113">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="a54a2-114">Azure SDK for Go をプロジェクトに含める</span><span class="sxs-lookup"><span data-stu-id="a54a2-114">Including the Azure SDK for Go in your project</span></span>

<span data-ttu-id="a54a2-115">Go コードから Azure サービスを使用するには、対話するサービスと必要な `autorest` モジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="a54a2-115">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="a54a2-116">提供されているモジュールの一覧については、[利用可能なサービス](https://godoc.org/github.com/Azure/azure-sdk-for-go)および [AutoRest パッケージ](https://godoc.org/github.com/Azure/go-autorest) の GoDoc を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a54a2-116">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="a54a2-117">必要となる `go-autorest` の最も一般的なパッケージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="a54a2-117">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="a54a2-118">パッケージ</span><span class="sxs-lookup"><span data-stu-id="a54a2-118">Package</span></span> | <span data-ttu-id="a54a2-119">[説明]</span><span class="sxs-lookup"><span data-stu-id="a54a2-119">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="a54a2-120">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="a54a2-120">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="a54a2-121">サービス クライアント認証を処理するためのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="a54a2-121">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="a54a2-122">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="a54a2-122">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="a54a2-123">Azure サービスと対話するための定数</span><span class="sxs-lookup"><span data-stu-id="a54a2-123">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="a54a2-124">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="a54a2-124">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="a54a2-125">Azure サービスにアクセスするための認証メカニズム</span><span class="sxs-lookup"><span data-stu-id="a54a2-125">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="a54a2-126">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="a54a2-126">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="a54a2-127">Azure SDK のデータ構造を操作するための型アサーション ヘルパー</span><span class="sxs-lookup"><span data-stu-id="a54a2-127">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="a54a2-128">Azure サービス用のモジュールは、SDK API とは別にバージョン管理されています。</span><span class="sxs-lookup"><span data-stu-id="a54a2-128">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="a54a2-129">これらのバージョンは、モジュールのインポート パスに含まれ、_サービス バージョン_または_プロファイル_のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="a54a2-129">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="a54a2-130">現時点では、特定のサービス バージョンを開発とリリースの両方に使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a54a2-130">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="a54a2-131">サービスは、`services` モジュールの下にあります。</span><span class="sxs-lookup"><span data-stu-id="a54a2-131">Services are located under the `services` module.</span></span> <span data-ttu-id="a54a2-132">インポートの完全なパスは、サービス名の後に `YYYY-MM-DD` 形式のバージョンが続き、その後にもう一度サービス名が続きます。</span><span class="sxs-lookup"><span data-stu-id="a54a2-132">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="a54a2-133">たとえば、Compute サービスの `2017-03-30` バージョンを含めるには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="a54a2-133">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="a54a2-134">他のバージョンを使用する理由がない限り、現時点ではサービスの最新バージョンを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a54a2-134">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="a54a2-135">サービスの一括スナップショットが必要な場合は、単一のプロファイル バージョンを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="a54a2-135">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="a54a2-136">現時点では、ロックされたプロファイルはバージョン `2017-03-30` だけです。このバージョンには、サービスの最新の機能が含まれていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a54a2-136">Right now, the only locked profile is version `2017-03-30`, which may not have the latest features of services.</span></span> <span data-ttu-id="a54a2-137">プロファイルは `profiles` モジュールの下にあり、バージョンは `YYYY-MM-DD` 形式です。</span><span class="sxs-lookup"><span data-stu-id="a54a2-137">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="a54a2-138">サービスは、プロファイル バージョンの下にグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="a54a2-138">Services are grouped under their profile version.</span></span> <span data-ttu-id="a54a2-139">たとえば、`2017-03-09` プロファイルから Azure リソース管理モジュールをインポートするには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="a54a2-139">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="a54a2-140">`preview`、`latest` の各プロファイルも用意されています。</span><span class="sxs-lookup"><span data-stu-id="a54a2-140">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="a54a2-141">これらは使用しないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a54a2-141">Using them is not recommended.</span></span> <span data-ttu-id="a54a2-142">これらのプロファイルはローリング バージョンであり、サービスの動作が変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a54a2-142">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a54a2-143">次の手順</span><span class="sxs-lookup"><span data-stu-id="a54a2-143">Next steps</span></span>

<span data-ttu-id="a54a2-144">Azure SDK for Go を使い始めるときには、クイック スタートをお試しください。</span><span class="sxs-lookup"><span data-stu-id="a54a2-144">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="a54a2-145">テンプレートから仮想マシンをデプロイする</span><span class="sxs-lookup"><span data-stu-id="a54a2-145">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="a54a2-146">Azure Blob SDK for Go を使用して Azure Blob Storage にオブジェクトを転送する</span><span class="sxs-lookup"><span data-stu-id="a54a2-146">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="a54a2-147">Azure Database for PostgreSQL に接続する</span><span class="sxs-lookup"><span data-stu-id="a54a2-147">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="a54a2-148">Go SDK で他のサービスをすぐに使用する場合は、用意されているサンプル コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a54a2-148">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="a54a2-149">Azure サービスによる認証</span><span class="sxs-lookup"><span data-stu-id="a54a2-149">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="a54a2-150">SSH 認証を使用した新しい仮想マシンのデプロイ</span><span class="sxs-lookup"><span data-stu-id="a54a2-150">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="a54a2-151">Azure Container Instances へのコンテナー イメージのデプロイ</span><span class="sxs-lookup"><span data-stu-id="a54a2-151">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="a54a2-152">Azure Kubernetes サービスでのクラスターの作成</span><span class="sxs-lookup"><span data-stu-id="a54a2-152">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="a54a2-153">Azure Storage サービスの操作</span><span class="sxs-lookup"><span data-stu-id="a54a2-153">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="a54a2-154">Azure SDK for Go のすべてのサンプル</span><span class="sxs-lookup"><span data-stu-id="a54a2-154">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)