---
title: Go 開発者向けのツール
description: Azure SDK for Go および Azure サービスを操作するためのツール
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 006d140bffb66fdd769a14511232d4ea5081811d
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066984"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="c27a0-103">Azure SDK for Go を使用する開発者向けのツール</span><span class="sxs-lookup"><span data-stu-id="c27a0-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="c27a0-104">Go コードを効果的に記述し、Azure サービスでシームレスに動作させるための推奨ツールを紹介します。</span><span class="sxs-lookup"><span data-stu-id="c27a0-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="c27a0-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c27a0-105">Azure CLI 2.0</span></span>

<span data-ttu-id="c27a0-106">Azure CLI 2.0 は、サブスクリプションに Azure リソースを作成して構成するためのコマンド ライン インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="c27a0-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="c27a0-107">CLI を使用すると、一般的な共有 Azure リソースの作成を速やかに開始できるため、サービスのより複雑な用途に集中できます。</span><span class="sxs-lookup"><span data-stu-id="c27a0-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="c27a0-108">CLI はクエリとフィルターの機能を備えているので、好みのコマンド ライン ツールに出力を直接パイプ処理できます。</span><span class="sxs-lookup"><span data-stu-id="c27a0-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="c27a0-109">CLI はローカル システムにインストールすることも、Docker イメージとしてインストールすることもできます。また、[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を介してインストールすることも可能です。</span><span class="sxs-lookup"><span data-stu-id="c27a0-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c27a0-110">Azure CLI 2.0 をインストールします</span><span class="sxs-lookup"><span data-stu-id="c27a0-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="c27a0-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c27a0-111">Visual Studio Code</span></span>

<span data-ttu-id="c27a0-112">Visual Studio Code は、拡張機能を通じて Go 言語を包括的にサポートする軽量エディターです。</span><span class="sxs-lookup"><span data-stu-id="c27a0-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="c27a0-113">これらの拡張機能により、オートコンプリート、`impl` テンプレート、リファクタリング、デバッグなどの機能のサポートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="c27a0-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="c27a0-114">Visual Studio Code では、ソース管理などの一般的な開発者ツールの多数の拡張機能に加え、Azure サービスと直接対話するための拡張機能も提供されます。</span><span class="sxs-lookup"><span data-stu-id="c27a0-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="c27a0-115">Microsoft は、Azure CLI の対話型インターフェイスなど、これらの Azure 拡張機能を含む公式のメタ拡張機能を管理しています。</span><span class="sxs-lookup"><span data-stu-id="c27a0-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="c27a0-116">Visual Studio Code をインストールする</span><span class="sxs-lookup"><span data-stu-id="c27a0-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="c27a0-117">Visual Studio Code Go 拡張機能を入手する</span><span class="sxs-lookup"><span data-stu-id="c27a0-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="c27a0-118">Azure Tools 拡張機能を入手する</span><span class="sxs-lookup"><span data-stu-id="c27a0-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="c27a0-119">dep を使用した依存関係の管理</span><span class="sxs-lookup"><span data-stu-id="c27a0-119">Dependency management with dep</span></span>

<span data-ttu-id="c27a0-120">公式ソリューションはまだありませんが、パッケージの依存関係を管理し、Go でベンダリングするさまざまな方法があります。</span><span class="sxs-lookup"><span data-stu-id="c27a0-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="c27a0-121">この管理を行うときには、`dep` 依存関係マネージャーを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c27a0-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="c27a0-122">Azure SDK for Go ではベンダリングに dep を使用します。dep を使用して、他のプロジェクトの依存関係を正しく取得することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="c27a0-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c27a0-123">dep 依存関係マネージャーを入手する</span><span class="sxs-lookup"><span data-stu-id="c27a0-123">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
