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
<span data-ttu-id="c63bc-101">[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) は、Go バージョン 1.8 以降と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="c63bc-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="c63bc-102">[Azure Stack プロファイル](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles)を使用している環境では、Go バージョン 1.9 が最小要件となります。</span><span class="sxs-lookup"><span data-stu-id="c63bc-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="c63bc-103">システム上に Go がない場合は、[Go のインストール手順](https://golang.org/doc/install)に従ってください。</span><span class="sxs-lookup"><span data-stu-id="c63bc-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="c63bc-104">`go get` を使用して、Azure SDK for Go と依存関係を入手できます。</span><span class="sxs-lookup"><span data-stu-id="c63bc-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="c63bc-105">URL では `Azure` の先頭文字を必ず大文字にしてください。</span><span class="sxs-lookup"><span data-stu-id="c63bc-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="c63bc-106">そうしないと、SDK を使用するときに大文字と小文字の区別に関連するインポートの問題が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c63bc-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="c63bc-107">インポート ステートメントでも `Azure` の先頭文字を大文字にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c63bc-107">You also need to capitalize `Azure` in your import statements.</span></span>

