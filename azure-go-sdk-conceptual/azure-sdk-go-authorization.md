---
title: Azure SDK for Go での認証
description: Azure SDK for Go で使用できる認証方法とそれらの使用方法について説明します。
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.service: active-directory
ms.component: authentication
ms.openlocfilehash: 8f94b9ba715c32263d324306cce69bd484c05702
ms.sourcegitcommit: c435f6602524565d340aac5506be5e955e78f16c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2018
ms.locfileid: "44711976"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Azure SDK for Go における認証方法

Azure SDK for Go には、Azure での認証方法が複数用意されています。 これらの認証の "_種類_" は、さまざまな認証 "_方法_" で呼び出されます。 この記事では、使用可能な種類、方法、およびアプリケーションに最適なものを選択する方法について説明します。

## <a name="available-authentication-types-and-methods"></a>使用可能な認証の種類と方法

Azure SDK for Go では、異なる資格情報セットを使用する複数の認証の種類が提供されます。 認証の種類は、それぞれ異なる認証方法 (SDK が資格情報を入力として受け取る方法) で使用できます。 次の表に、使用できる認証の種類と、アプリケーションで使用する際に推奨される状況を示します。

| 認証の種類 | 推奨される状況 |
|---------------------|---------------------|
| 証明書ベースの認証 | Azure Active Directory (AAD) ユーザーまたはサービス プリンシパル用に構成された X509 証明書がある場合。 詳細については、「[Azure Active Directory の証明書ベースの認証の概要]」をご覧ください。 |
| クライアントの資格情報 | このアプリケーションまたはアプリケーションが属するクラス用に設定された構成済みのサービス プリンシパルがある場合。 詳細については、[Azure CLI でのサービス プリンシパルの作成]に関する記事をご覧ください。 |
| Azure リソースのマネージド ID | マネージド ID で構成された Azure リソース上でアプリケーションが実行されている場合。 詳細については、[Azure リソースのマネージド ID]に関するページをご覧ください。 |
| デバイス トークン | お使いのアプリケーションでは、対話形式 "__のみ__" の使用が想定されている場合。 ユーザーが多要素認証を有効にしていることがあります。 ユーザーは、Web ブラウザーにアクセスしてサインインできます。 詳細については、「[デバイス トークン認証を使用する](#use-device-token-authentication)」をご覧ください。|
| ユーザー名/パスワード | 他のどの認証方法も使用できない対話型アプリケーションがある場合。 ユーザーは、AAD サインインで多要素認証を有効にすることはできません。 |

> [!IMPORTANT]
> クライアントの資格情報以外の認証の種類を使用する場合は、アプリケーションを Azure Active Directory に登録する必要があります。 方法については、「[Azure Active Directory とアプリケーションの統合](/azure/active-directory/develop/active-directory-integrating-applications)」をご覧ください。
>
> [!NOTE]
> 特別な要件がない限り、ユーザー名/パスワード認証は避けてください。 ユーザー ベースのサインインが適している場合、通常はデバイス トークン認証を代わりに使用できます。

[Azure Active Directory の証明書ベースの認証の概要]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Azure CLI でのサービス プリンシパルの作成]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure リソースのマネージド ID]: /azure/active-directory/managed-identities-azure-resources/overview

これらの認証の種類は、異なる方法で使用できます。

* [_環境ベースの認証_](#use-environment-based-authentication)では、プログラムの環境から資格情報を直接読み取ります。
* [_ファイル ベースの認証_](#use-file-based-authentication)では、サービス プリンシパルの資格情報が含まれたファイルを読み込みます。
* "[_クライアント ベースの認証_](#use-an-authentication-client)" では、コード内のオブジェクトを使用します。プログラムの実行時に資格情報を提供する必要があります。
* "[_デバイス トークン認証_](#use-device-token-authentication)" では、ユーザーはトークンを使用して Web ブラウザーを介して対話形式でサインインする必要があります。

認証の関数と型はすべて `github.com/Azure/go-autorest/autorest/azure/auth` パッケージで使用できます。

> [!NOTE]
> 特別な要件がない限り、クライアント ベースの認証は避けてください。 この認証方法では、バッド プラクティスが助長されます。 特に、クライアント ベースの認証を使用すると、資格情報をハードコードしてしまいがちになります。 また、認証用のカスタム コードを記述すると、認証要件が変わった場合に、SDK の今後のリリースでコードが動作しなくなる可能性があります。

## <a name="use-environment-based-authentication"></a>環境ベースの認証を使用する

管理された設定でアプリケーションを実行している場合は、環境ベースの認証が自然な選択となります。 この認証方法を使用して、シェル環境を構成してからアプリケーションを実行します。 実行時に、Azure で認証するために、これらの環境変数が Go SDK によって読み取られます。

環境ベースの認証では、デバイス トークンを除くすべての認証方法がサポートされ、次の順番で評価されます。

* クライアントの資格情報
* X509 証明書
* ユーザー名/パスワード
* Azure リソースのマネージド ID

認証の種類に設定されていない値があったり、拒否されたりした場合は、SDK では自動的に次の認証の種類が試行されます。 試行できる種類がそれ以上ない場合、SDK によってエラーが返されます。

次の表に、環境ベースの認証でサポートされている各認証の種類で設定する必要がある環境変数の詳細を示します。

| 認証の種類 | 環境変数 | 説明 |
| ------------------- | -------------------- | ----------- |
| __クライアントの資格情報__ | `AZURE_TENANT_ID` | サービス プリンシパルが属する Active Directory テナントの ID。 |
| | `AZURE_CLIENT_ID` | サービス プリンシパルの名前または ID。 |
| | `AZURE_CLIENT_SECRET` | サービス プリンシパルに関連付けられているシークレット。 |
| __証明書__ | `AZURE_TENANT_ID` | 証明書が登録されている Active Directory テナントの ID。 |
| | `AZURE_CLIENT_ID` | 証明書に関連付けられているアプリケーション クライアント ID。 |
| | `AZURE_CERTIFICATE_PATH` | クライアント証明書ファイルのパス。 |
| | `AZURE_CERTIFICATE_PASSWORD` | クライアント証明書のパスワード。 |
| __ユーザー名/パスワード__ | `AZURE_TENANT_ID` | ユーザーが属する Active Directory テナントの ID。 |
| | `AZURE_CLIENT_ID` | アプリケーション クライアント ID。 |
| | `AZURE_USERNAME` | サインインに使用するユーザー名。 |
| | `AZURE_PASSWORD` | サインインに使用するパスワード。 |
| __管理対象 ID__ | | マネージド ID 認証では資格情報は必要ありません。 アプリケーションは、マネージド ID を使用するように構成された Azure リソース上で実行されている必要があります。 詳細については、[Azure リソースのマネージド ID]に関するページをご覧ください。 |

既定の Azure パブリック クラウド以外のクラウドまたは管理エンドポイントに接続するには、次の環境変数を設定します。 Azure Stack、異なる地理的リージョンのクラウド、またはクラシック デプロイ モデルを使用する場合が最も一般的な理由です。

| 環境変数 | 説明  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | 接続先のクラウド環境の名前。 |
| `AZURE_AD_RESOURCE` | 接続するときに管理エンドポイントの URI として使用する Active Directory リソース ID。 |

環境ベースの認証を使用する場合は、[NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) 関数を呼び出して authorizer オブジェクトを取得します。 その後、このオブジェクトがクライアントの `Authorizer` プロパティで設定され、Azure へのクライアントのアクセスが許可されます。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Azure Stack での認証

Azure Stack で認証するには、次の変数を設定する必要があります。

| 環境変数 | 説明  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Active Directory エンドポイント。 |
| `AZURE_AD_RESOURCE` | Active Directory リソース ID。 |

これらの変数は、Azure Stack のメタデータ情報から取得できます。 メタデータを取得するには、Azure Stack 環境で Web ブラウザーを開き、URL として `(ResourceManagerURL)/metadata/endpoints?api-version=1.0` を使用します。

`ResourceManagerURL` は、Azure Stack デプロイのリージョン名、マシン名、外部完全修飾ドメイン名 (FQDN) によって異なります。

| 環境 | ResourceManagerURL |
|----------------------|--------------|
| 開発キット | `https://management.local.azurestack.external/` |
| 統合システム | `https://management.(region).ext-(machine-name).(FQDN)` |

Azure Stack での Azure SDK for Go の使用方法の詳細については、「[Azure Stack での GO による API バージョンのプロファイルの使用](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)」をご覧ください。

## <a name="use-file-based-authentication"></a>ファイル ベースの認証を使用する

ファイルベースの認証では、[Azure CLI](/cli/azure) によって生成されるファイル形式を使用します。 `--sdk-auth` パラメーターを指定して新しいサービス プリンシパルを作成すると、このファイルを簡単に作成できます。 ファイル ベースの認証を使用する場合は、サービス プリンシパルの作成時に、この引数が指定されていることを確認してください。 CLI では出力が `stdout` に生成されるので、出力をファイルにリダイレクトします。

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

`AZURE_AUTH_LOCATION` 環境変数を、承認ファイルがある場所に設定します。 この環境変数をアプリケーションが読み取り、環境変数内の資格情報が解析されます。 承認ファイルを実行時に選択する必要がある場合は、[os.Setenv](https://golang.org/pkg/os/#Setenv) 関数を使用してプログラムの環境を操作します。

認証情報を読み込むには、[NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) 関数を呼び出します。 環境ベースの承認とは異なり、ファイル ベースの承認にはリソース エンドポイントが必要です。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

サービス プリンシパルの使用とアクセス許可の管理の詳細については、[Azure CLI でのサービス プリンシパルの作成]に関する記事をご覧ください。

## <a name="use-device-token-authentication"></a>デバイス トークン認証を使用する

ユーザーが対話形式でサインインできるようにする場合、最良の方法は、デバイス トークン認証を使用することです。 この認証フローでは、Microsoft サインイン サイトに貼り付けるトークンをユーザーに渡し、ユーザーはこのサイトで Azure Active Directory (AAD) アカウントで認証します。 標準のユーザー名/パスワード認証とは異なり、この認証方法では多要素認証が有効になっているアカウントがサポートされます。

デバイス トークン認証を使用するには、[NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) 関数で [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) Authorizer を作成します。 作成されたオブジェクトで [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) を呼び出して認証プロセスを開始します。 デバイス認証フローでは、認証フロー全体が完了するまでプログラムの実行がブロックされます。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>認証クライアントを使用する

特定の種類の認証が必要であり、ユーザーの認証情報を読み込む処理をプログラムで実行する場合は、[auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) インターフェイスに準拠したクライアントを使用できます。 次の場合にこのインターフェイスを実装する種類を使用します。

* 対話型プログラムを記述する
* 特別な構成ファイルを使用する
* 組み込みの認証方法を使用できないようにする要件がある

> [!WARNING]
> Azure の資格情報をアプリケーションにハードコードしないでください。 シークレットをアプリケーションのバイナリに配置すると、アプリケーションが実行されているかどうかに関係なく、攻撃者がシークレットを簡単に抽出できるようになります。 この場合、資格情報が承認されているすべての Azure リソースが危険にさらされます。

次の表に、`AuthorizerConfig` インターフェイスに準拠した SDK の型を示します。

| 認証の種類 | Authorizer の型 |
|---------------------|-----------------------|
| 証明書ベースの認証 | [ClientCertificateConfig] |
| クライアントの資格情報 | [ClientCredentialsConfig] |
| Azure リソースのマネージド ID | [MSIConfig] |
| ユーザー名/パスワード | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

関連付けられた `New` 関数で認証子を作成し、作成されたオブジェクトで `Authorize` を呼び出して認証します。 たとえば、証明書ベースの認証を使用する場合は次のようになります。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
