---
title: "Go 開発者向けのツール"
description: "Azure SDK for Go および Azure サービスを操作するためのツール"
keywords: azure, go, golang, azure, visual studio, visual studio code
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="aa4fe-104">Azure SDK for Go を使用する開発者向けのツール</span><span class="sxs-lookup"><span data-stu-id="aa4fe-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="aa4fe-105">Go コードを効果的に記述し、Azure サービスでシームレスに動作させるための推奨ツールを紹介します。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="aa4fe-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="aa4fe-106">Azure CLI 2.0</span></span>

<span data-ttu-id="aa4fe-107">Azure CLI 2.0 は、サブスクリプションに Azure リソースを作成して構成するためのコマンド ライン インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="aa4fe-108">CLI を使用すると、一般的な共有 Azure リソースの作成を速やかに開始できるため、サービスのより複雑な用途に集中できます。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="aa4fe-109">CLI はクエリとフィルターの機能を備えているので、好みのコマンド ライン ツールに出力を直接パイプ処理できます。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="aa4fe-110">CLI はローカル システムにインストールすることも、Docker イメージとしてインストールすることもできます。また、[Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) を介してインストールすることも可能です。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="aa4fe-111">Azure CLI 2.0 をインストールします</span><span class="sxs-lookup"><span data-stu-id="aa4fe-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="aa4fe-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aa4fe-112">Visual Studio Code</span></span>

<span data-ttu-id="aa4fe-113">Visual Studio Code は、拡張機能を通じて Go 言語を包括的にサポートする軽量エディターです。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="aa4fe-114">これらの拡張機能により、オートコンプリート、`impl` テンプレート、リファクタリング、デバッグなどの機能のサポートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="aa4fe-115">Visual Studio Code では、ソース管理などの一般的な開発者ツールの多数の拡張機能に加え、Azure サービスと直接対話するための拡張機能も提供されます。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="aa4fe-116">Microsoft は、Azure CLI の対話型インターフェイスなど、これらの Azure 拡張機能を含む公式のメタ拡張機能を管理しています。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="aa4fe-117">Visual Studio Code をインストールする</span><span class="sxs-lookup"><span data-stu-id="aa4fe-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="aa4fe-118">Visual Studio Code Go 拡張機能を入手する</span><span class="sxs-lookup"><span data-stu-id="aa4fe-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="aa4fe-119">Azure Tools 拡張機能を入手する</span><span class="sxs-lookup"><span data-stu-id="aa4fe-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="aa4fe-120">dep を使用した依存関係の管理</span><span class="sxs-lookup"><span data-stu-id="aa4fe-120">Dependency management with dep</span></span>

<span data-ttu-id="aa4fe-121">公式ソリューションはまだありませんが、パッケージの依存関係を管理し、Go でベンダリングするさまざまな方法があります。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="aa4fe-122">この管理を行うときには、`dep` 依存関係マネージャーを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="aa4fe-123">Azure SDK for Go ではベンダリングに dep を使用します。dep を使用して、他のプロジェクトの依存関係を正しく取得することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="aa4fe-124">dep 依存関係マネージャーを入手する</span><span class="sxs-lookup"><span data-stu-id="aa4fe-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="aa4fe-125">Application Insights のテレメトリ</span><span class="sxs-lookup"><span data-stu-id="aa4fe-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="aa4fe-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) は、アプリケーションからテレメトリ情報を簡単に収集できる分析製品であり、Azure エコシステム、Visual Studio Team Services、GitHub と統合されています。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="aa4fe-127">このツールはさまざまなアプリケーションで使用できます。Microsoft は、Application Insights で動作する Go SDK を提供しています。</span><span class="sxs-lookup"><span data-stu-id="aa4fe-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="aa4fe-128">Application Insights SDK for Go を入手する</span><span class="sxs-lookup"><span data-stu-id="aa4fe-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
