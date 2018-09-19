---
title: Azure SDK for Go を使用する開発者向けのツール
description: Azure SDK for Go および Azure サービスを操作するためのツール
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059205"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="6d6b2-103">Azure SDK for Go を使用する開発者向けのツール</span><span class="sxs-lookup"><span data-stu-id="6d6b2-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="6d6b2-104">Go コードを効果的に記述し、Azure サービスでシームレスに動作させるための推奨ツールを紹介します。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="6d6b2-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6d6b2-105">Azure CLI</span></span>

<span data-ttu-id="6d6b2-106">Azure CLI は、サブスクリプションで Azure リソースを作成して構成するためのコマンド ライン インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="6d6b2-107">CLI を使用すると、一般的な共有 Azure リソースの作成を速やかに開始できるため、サービスのより複雑な用途に集中できます。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="6d6b2-108">CLI はクエリとフィルターの機能を備えているので、好みのコマンド ライン ツールに出力を直接パイプ処理できます。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="6d6b2-109">CLI はローカル システムにインストールすることも、Docker イメージとしてインストールすることもできます。また、[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を介してインストールすることも可能です。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6d6b2-110">Azure CLI のインストール</span><span class="sxs-lookup"><span data-stu-id="6d6b2-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="6d6b2-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d6b2-111">Visual Studio Code</span></span>

<span data-ttu-id="6d6b2-112">Visual Studio Code は、Go をサポートする軽量なエディターです。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="6d6b2-113">この拡張機能により、オートコンプリート、`impl` テンプレート、リファクタリング、デバッグなどの機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="6d6b2-114">Visual Studio Code では、エディター内からソース管理にアクセスすることが可能で、Azure サービスを操作するための拡張機能も提供されています。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="6d6b2-115">Visual Studio Code をインストールする</span><span class="sxs-lookup"><span data-stu-id="6d6b2-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="6d6b2-116">Visual Studio Code Go 拡張機能を入手する</span><span class="sxs-lookup"><span data-stu-id="6d6b2-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="6d6b2-117">Visual Studio Code の Azure ツール拡張機能を入手する</span><span class="sxs-lookup"><span data-stu-id="6d6b2-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="6d6b2-118">Azure DevOps プロジェクトによる CI/CD</span><span class="sxs-lookup"><span data-stu-id="6d6b2-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="6d6b2-119">Azure DevOps プロジェクトのパイプラインを使用すると、Go アプリケーションの継続的インテグレーション システムを設定できます。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="6d6b2-120">Git リポジトリだけで、Azure 上で直接デプロイとテストを実行できます。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6d6b2-121">Azure DevOps Projects で CI/CD パイプラインを作成する方法を確認する</span><span class="sxs-lookup"><span data-stu-id="6d6b2-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="6d6b2-122">dep を使用した依存関係の管理</span><span class="sxs-lookup"><span data-stu-id="6d6b2-122">Dependency management with dep</span></span>

<span data-ttu-id="6d6b2-123">Azure SDK for Go では、依存関係の管理に dep を使用します。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="6d6b2-124">dep コマンドを使用すると、Go アプリケーションのベンダー要件をプルし、バージョン間の競合を回避し、プロジェクトが適切に動作することを確認できます。</span><span class="sxs-lookup"><span data-stu-id="6d6b2-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6d6b2-125">dep 依存関係マネージャーを入手する</span><span class="sxs-lookup"><span data-stu-id="6d6b2-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
