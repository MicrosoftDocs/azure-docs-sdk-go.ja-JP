---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 0febc2cc42ad95a1b7a5032c7987e37cc82f374e
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067035"
---
[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) は、Go バージョン 1.8 以降と互換性があります。 [Azure Stack プロファイル](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles)を使用している環境では、Go バージョン 1.9 が最小要件となります。
システム上に Go がない場合は、[Go のインストール手順](https://golang.org/doc/install)に従ってください。

`go get` を使用して、Azure SDK for Go と依存関係を入手できます。

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> URL では `Azure` の先頭文字を必ず大文字にしてください。 そうしないと、SDK を使用するときに大文字と小文字の区別に関連するインポートの問題が発生する可能性があります。 インポート ステートメントでも `Azure` の先頭文字を大文字にする必要があります。

