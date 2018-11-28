---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2018
ms.locfileid: "52293510"
---
[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) は、Go バージョン 1.8 以上と互換性があります。 [Azure Stack プロファイル](/azure/azure-stack/user/azure-stack-version-profiles-go)を使用している環境では、Go バージョン 1.9 が最小要件となります。
Go をインストールする必要がある場合は、[Go のインストール手順](https://golang.org/doc/install)に従ってください。

`go get` を使用して、Azure SDK for Go と依存関係をダウンロードできます。

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> URL では `Azure` の先頭文字を必ず大文字にしてください。 そうしないと、SDK を使用するときに大文字と小文字の区別に関連するインポートの問題が発生する可能性があります。 インポート ステートメントでも `Azure` の先頭文字を大文字にする必要があります。
