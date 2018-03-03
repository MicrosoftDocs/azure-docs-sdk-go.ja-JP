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
# <a name="installing-the-azure-sdk-for-go"></a>Azure SDK for Go のインストール

Azure SDK for Go へようこそ。 この SDK を使用すると、Go アプリケーションから Azure サービスを管理し、サービスと対話できます。

## <a name="get-the-azure-sdk-for-go"></a>Azure SDK for Go を入手する

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Azure Storage Blob を操作するには、別の SDK が必要です。

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a>Azure SDK for Go をベンダリングする

Azure SDK for Go は、[dep](https://github.com/golang/dep) を使用してベンダリングできます。 安定性のため、ベンダリングすることをお勧めします。 `dep` サポートを使用するには、`Gopkg.toml` の `[[constraint]]` セクションに `github.com/Azure/azure-sdk-for-go` を追加します。 たとえば、バージョン `14.0.0` でベンダリングするには、次のエントリを追加します。

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a>Azure SDK for Go をプロジェクトに含める

Go コードから Azure サービスを使用するには、対話するサービスと必要な `autorest` モジュールをインポートします。
提供されているモジュールの一覧については、[利用可能なサービス](https://godoc.org/github.com/Azure/azure-sdk-for-go)および [AutoRest パッケージ](https://godoc.org/github.com/Azure/go-autorest) の GoDoc を参照してください。 必要となる `go-autorest` の最も一般的なパッケージを次に示します。

| パッケージ | [説明] |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | サービス クライアント認証を処理するためのオブジェクト |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Azure サービスと対話するための定数 |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Azure サービスにアクセスするための認証メカニズム |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Azure SDK のデータ構造を操作するための型アサーション ヘルパー |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Azure サービス用のモジュールは、SDK API とは別にバージョン管理されています。 これらのバージョンは、モジュールのインポート パスに含まれ、_サービス バージョン_または_プロファイル_のいずれかになります。 現時点では、特定のサービス バージョンを開発とリリースの両方に使用することをお勧めします。 サービスは、`services` モジュールの下にあります。 インポートの完全なパスは、サービス名の後に `YYYY-MM-DD` 形式のバージョンが続き、その後にもう一度サービス名が続きます。 たとえば、Compute サービスの `2017-03-30` バージョンを含めるには、次のように入力します。

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

他のバージョンを使用する理由がない限り、現時点ではサービスの最新バージョンを使用することをお勧めします。

サービスの一括スナップショットが必要な場合は、単一のプロファイル バージョンを選択することもできます。 現時点では、ロックされたプロファイルはバージョン `2017-03-30` だけです。このバージョンには、サービスの最新の機能が含まれていない可能性があります。 プロファイルは `profiles` モジュールの下にあり、バージョンは `YYYY-MM-DD` 形式です。 サービスは、プロファイル バージョンの下にグループ化されます。 たとえば、`2017-03-09` プロファイルから Azure リソース管理モジュールをインポートするには、次のように入力します。

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> `preview`、`latest` の各プロファイルも用意されています。 これらは使用しないことをお勧めします。 これらのプロファイルはローリング バージョンであり、サービスの動作が変更される可能性があります。

## <a name="next-steps"></a>次の手順

Azure SDK for Go を使い始めるときには、クイック スタートをお試しください。

* [テンプレートから仮想マシンをデプロイする](azure-sdk-go-qs-vm.md)
* [Azure Blob SDK for Go を使用して Azure Blob Storage にオブジェクトを転送する](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Azure Database for PostgreSQL に接続する](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Go SDK で他のサービスをすぐに使用する場合は、用意されているサンプル コードを参照してください。

* [Azure サービスによる認証](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [SSH 認証を使用した新しい仮想マシンのデプロイ](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Azure Container Instances へのコンテナー イメージのデプロイ](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Azure Kubernetes サービスでのクラスターの作成](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Azure Storage サービスの操作](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Azure SDK for Go のすべてのサンプル](https://github.com/azure-samples/azure-sdk-for-go-samples)
