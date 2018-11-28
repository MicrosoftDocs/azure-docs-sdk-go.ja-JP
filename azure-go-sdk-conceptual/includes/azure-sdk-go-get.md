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
<span data-ttu-id="060d1-101">[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) は、Go バージョン 1.8 以上と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="060d1-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="060d1-102">[Azure Stack プロファイル](/azure/azure-stack/user/azure-stack-version-profiles-go)を使用している環境では、Go バージョン 1.9 が最小要件となります。</span><span class="sxs-lookup"><span data-stu-id="060d1-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="060d1-103">Go をインストールする必要がある場合は、[Go のインストール手順](https://golang.org/doc/install)に従ってください。</span><span class="sxs-lookup"><span data-stu-id="060d1-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="060d1-104">`go get` を使用して、Azure SDK for Go と依存関係をダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="060d1-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="060d1-105">URL では `Azure` の先頭文字を必ず大文字にしてください。</span><span class="sxs-lookup"><span data-stu-id="060d1-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="060d1-106">そうしないと、SDK を使用するときに大文字と小文字の区別に関連するインポートの問題が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="060d1-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="060d1-107">インポート ステートメントでも `Azure` の先頭文字を大文字にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="060d1-107">You also need to capitalize `Azure` in your import statements.</span></span>
