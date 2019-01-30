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
ms.openlocfilehash: 2799e3a6c637036eeaf7b20adf8aa55a8a4ab400
ms.sourcegitcommit: 4db332f5e43a5b43032ff9017805d5fd5a650d86
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2019
ms.locfileid: "55145534"
---
# <a name="install-the-azure-sdk-for-go"></a>Azure SDK for Go のインストール

Azure SDK for Go へようこそ。 この SDK を使用すると、Go アプリケーションから Azure サービスを管理し、サービスと対話できます。

## <a name="get-the-azure-sdk-for-go"></a>Azure SDK for Go を入手する

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

一部の Azure サービスには独自の Go SDK が用意されていますが、これは Azure SDK for Go のコア パッケージには含まれていません。 次の表に、独自の SDK が用意されているサービスと、そのパッケージ名を示します。 これらのパッケージは、すべてプレビュー段階にあると考えられます。

| Service | Package |
|---------|---------|
| Blob Storage | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| File Storage | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| ストレージ キュー | [github.com/Azure/azure-storage-queue-go](https://github.com/Azure/azure-storage-queue-go) |
| イベント ハブ | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Service Bus | [github.com/Azure/azure-service-bus-go](https://github.com/Azure/azure-service-bus-go) |
| Application Insights | [github.com/Microsoft/ApplicationInsights-go](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>Azure SDK for Go をベンダリングする

Azure SDK for Go は、[dep](https://github.com/golang/dep) を使用してベンダリングできます。 安定性のため、ベンダリングすることをお勧めします。 独自のプロジェクトで `dep` を使用するには、`github.com/Azure/azure-sdk-for-go` を `Gopkg.toml` の `[[constraint]]` セクションに追加します。 たとえば、バージョン `14.0.0` でベンダリングするには、次のエントリを追加します。

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>Azure SDK for Go をプロジェクトに含める

Go コードから Azure サービスを使用するには、対話するサービスと必要な `autorest` モジュールをインポートします。
提供されているモジュールの一覧については、[利用可能なサービス](https://godoc.org/github.com/Azure/azure-sdk-for-go)および [AutoRest パッケージ](https://godoc.org/github.com/Azure/go-autorest) の GoDoc を参照してください。 必要となる `go-autorest` の最も一般的なパッケージを次に示します。

| Package | 説明 |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | サービス クライアント認証を処理するためのオブジェクト |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Azure サービスと対話するための定数 |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Azure サービスにアクセスするための認証メカニズム |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Azure SDK のデータ構造を操作するための型アサーション ヘルパー |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Go パッケージと Azure サービスは、個別にバージョン管理されます。 サービスのバージョンは、`services` モジュールの下の、モジュールのインポート パスに含まれます。 モジュールの完全なパスは、サービス名の後に `YYYY-MM-DD` 形式のバージョンが続き、その後にもう一度サービス名が続きます。 たとえば、Compute サービスの `2017-03-30` バージョンをインポートするには、次のように入力します。

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

開発の開始時には最新バージョンのサービスを使用して、一貫性を保つことをお勧めします。
バージョン間で Go SDK の更新プログラムが公開されない場合でも、その期間中にコードの破損につながりかねないようなサービス要件の変更が発生することがあるためです。

サービスの一括スナップショットが必要な場合は、単一のプロファイル バージョンを選択することもできます。 現時点では、ロックされたプロファイルはバージョン `2017-03-09` だけです。このバージョンには、サービスの最新の機能が含まれていない可能性があります。 プロファイルは `profiles` モジュールの下にあり、バージョンは `YYYY-MM-DD` 形式です。 サービスは、プロファイル バージョンの下にグループ化されます。 たとえば、`2017-03-09` プロファイルから Azure リソース管理モジュールをインポートするには、次のように入力します。

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

* [Azure サービスによる認証](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [SSH 認証を使用した新しい仮想マシンのデプロイ](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Azure Container Instances へのコンテナー イメージのデプロイ](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Azure Kubernetes Service でのクラスターの作成](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute)
* [Azure Storage サービスの操作](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Azure SDK for Go のすべてのサンプル](https://github.com/azure-samples/azure-sdk-for-go-samples)
