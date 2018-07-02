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
ms.openlocfilehash: 25b46e3a1636c39e261ba11c6f8939d8721cc693
ms.sourcegitcommit: 79d216c6b0442d0f3b3660ff2a34dc8b2049390c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093160"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Azure SDK for Go を使用する開発者向けのツール

Go コードを効果的に記述し、Azure サービスでシームレスに動作させるための推奨ツールを紹介します。

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure CLI 2.0 は、サブスクリプションに Azure リソースを作成して構成するためのコマンド ライン インターフェイスを提供します。 CLI を使用すると、一般的な共有 Azure リソースの作成を速やかに開始できるため、サービスのより複雑な用途に集中できます。 CLI はクエリとフィルターの機能を備えているので、好みのコマンド ライン ツールに出力を直接パイプ処理できます。 CLI はローカル システムにインストールすることも、Docker イメージとしてインストールすることもできます。また、[Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) を介してインストールすることも可能です。

> [!div class="nextstepaction"]
> [Azure CLI 2.0 をインストールします](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code は、拡張機能を通じて Go 言語を包括的にサポートする軽量エディターです。 これらの拡張機能により、オートコンプリート、`impl` テンプレート、リファクタリング、デバッグなどの機能のサポートが追加されます。 Visual Studio Code では、ソース管理などの一般的な開発者ツールの多数の拡張機能に加え、Azure サービスと直接対話するための拡張機能も提供されます。 Microsoft は、Azure CLI の対話型インターフェイスなど、これらの Azure 拡張機能を含む公式のメタ拡張機能を管理しています。

* [Visual Studio Code をインストールする](https://code.visualstudio.com/Download)
* [Visual Studio Code Go 拡張機能を入手する](https://code.visualstudio.com/docs/languages/go)
* [Azure Tools 拡張機能を入手する](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>dep を使用した依存関係の管理

公式ソリューションはまだありませんが、パッケージの依存関係を管理し、Go でベンダリングするさまざまな方法があります。 この管理を行うときには、`dep` 依存関係マネージャーを使用することをお勧めします。 Azure SDK for Go ではベンダリングに dep を使用します。dep を使用して、他のプロジェクトの依存関係を正しく取得することが保証されます。

> [!div class="nextstepaction"]
> [dep 依存関係マネージャーを入手する](https://github.com/golang/dep)
