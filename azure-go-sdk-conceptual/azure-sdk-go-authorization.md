---
title: Azure SDK for Go での認証
description: Azure SDK for Go で使用できる認証方法とそれらの使用方法について説明します。
services: azure
author: sptramer
ms.author: sttramer
ms.date: 04/03/2018
ms.topic: article
ms.service: azure
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 39f9dc5a7cdf9ab84cfd9264446bacb31392ca80
ms.sourcegitcommit: 59d2b4c9d8da15fbbd15e36551093219fdaf256e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Azure SDK for Go における認証方法

Azure SDK for Go では、アプリケーションで使用できるさまざまな認証の種類と方法が提供されます。 環境変数からの情報の取得から、Web ベースの対話型認証まで、さまざまな認証方法がサポートされています。 この記事では、この SDK で使用可能な認証の種類とそれらの使用方法について説明します。 また、アプリケーションに適した認証の種類を選択するためのベスト プラクティスについても説明します。

## <a name="available-authentication-types-and-methods"></a>使用可能な認証の種類と方法

Azure SDK for Go では、異なる資格情報セットを使用する複数の認証の種類が提供されます。 これらの認証の種類は、それぞれ異なる認証方法 (SDK が資格情報を入力として受け取る方法) で使用できます。 次の表に、使用できる認証の種類と、アプリケーションで使用する際に推奨される状況を示します。

| 認証の種類 | 推奨される状況 |
|---------------------|---------------------|
| 証明書ベースの認証 | Azure Active Directory (AAD) ユーザーまたはサービス プリンシパル用に構成された X509 証明書がある場合。 詳細については、「[Azure Active Directory の証明書ベースの認証の概要]」をご覧ください。 |
| クライアントの資格情報 | このアプリケーションまたはアプリケーションが属するクラス用に設定された構成済みのサービス プリンシパルがある場合。 詳細については、[Azure CLI 2.0 でのサービス プリンシパルの作成]に関する記事をご覧ください。 |
| 管理対象サービス ID (MSI) | 管理対象サービス ID (MSI) で構成された Azure リソース上でアプリケーションが実行されている場合。 詳細については、「[Azure リソースの管理対象サービス ID (MSI)]」をご覧ください。 |
| デバイス トークン | アプリケーションを対話形式で__のみ__使用し、複数の AAD テナントのさまざまなユーザーが使用する可能性がある場合。 ユーザーは、Web ブラウザーにアクセスしてログインできます。 詳細については、「[デバイス トークン認証を使用する](#use-device-token-authentication)」をご覧ください。|
| ユーザー名/パスワード | 他のどの認証方法も使用できない対話型アプリケーションがある場合。 ユーザーは、AAD ログインで多要素認証を有効にすることはできません。 |

> [!IMPORTANT]
> クライアントの資格情報以外の認証の種類を使用する場合は、アプリケーションを Azure Active Directory に登録する必要があります。 方法については、「[Azure Active Directory とアプリケーションの統合](/azure/active-directory/develop/active-directory-integrating-applications)」をご覧ください。

> [!NOTE]
> 特別な要件がない限り、ユーザー名/パスワード認証は避けてください。 ユーザー ベースのログインが適している場合、通常はデバイス トークン認証を代わりに使用できます。

[Azure Active Directory の証明書ベースの認証の概要]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Azure CLI 2.0 でのサービス プリンシパルの作成]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure リソースの管理対象サービス ID (MSI)]: /azure/active-directory/managed-service-identity/overview

これらの認証の種類は、異なる方法で使用できます。 [_環境ベースの認証_](#use-environment-based-authentication)では、プログラムの環境から資格情報を直接読み取ります。 [_ファイル ベースの認証_](#use-file-based-authentication)では、サービス プリンシパルの資格情報が含まれたファイルを読み込みます。 [_クライアント ベースの認証_](#use-an-authentication-client)では、Go コード内のオブジェクトを使用します。プログラムの実行時に資格情報を提供する必要があります。 最後に、[_デバイス トークン認証_](#use-device-token-authentication)では、ユーザーはトークンを使用して Web ブラウザーを介して対話形式でログインする必要があります。環境ベースまたはファイル ベースの認証では使用できません。

認証の関数と型はすべて `github.com/Azure/go-autorest/autorest/azure/auth` パッケージで使用できます。

> [!NOTE]
> 特別な要件がない限り、クライアント ベースの認証は避けてください。 この認証方法では、バッド プラクティスが助長されます。 特に、クライアント ベースの認証を使用すると、資格情報をハードコードしてしまいがちになります。 また、認証用のカスタム コードを記述すると、認証要件が変わった場合に、SDK の今後のリリースでコードが動作しなくなる可能性があります。

## <a name="use-environment-based-authentication"></a>環境ベースの認証を使用する

コンテナーなどの厳しく管理された環境でアプリケーションを実行する場合は、環境ベースの認証が自然な選択となります。 アプリケーションを実行する前にシェル環境を構成すると、実行時に Go SDK によってこれらの環境変数が読み取られ、Azure で認証が行われます。 

環境ベースの認証では、デバイス トークンを除くすべての認証方法がサポートされ、クライアントの資格情報、証明書、ユーザー名/パスワード、管理対象サービス ID (MSI) の順番で評価されます。 必要な環境変数が設定されていない場合、または SDK が認証サービスから拒否された場合は、次の認証の種類が試されます。 SDK が環境から認証できない場合は、エラーが返されます。

次の表に、環境ベースの認証でサポートされている各認証の種類で設定する必要がある環境変数の詳細を示します。

| 認証の種類 | 環境変数 | [説明] |
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
| | `AZURE_USERNAME` | ログインに使用するユーザー名。 |
| | `AZURE_PASSWORD` | ログインに使用するパスワード。 |
| __MSI__ | | MSI では、資格情報を設定する必要はありません。 アプリケーションは、MSI を使用するように構成された Azure リソース上で実行されている必要があります。 詳細については、「[Azure リソースの管理対象サービス ID (MSI)]」をご覧ください。 |

既定の Azure パブリック クラウド以外のクラウドまたは管理エンドポイントに接続する必要がある場合は、次の環境変数を設定することもできます。 これらの環境変数は、Azure Stack、異なる地理的リージョンのクラウド、または Azure クラシック デプロイ モデルを使用する場合に設定するのが最も一般的です。

| 環境変数 | [説明]  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | 接続先のクラウド環境の名前。 |
| `AZURE_AD_RESOURCE` | 接続時に使用する Active Directory リソース ID。 これは、管理エンドポイントを参照する URI である必要があります。 |

環境ベースの認証を使用する場合は、[NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) 関数を呼び出して authorizer オブジェクトを取得します。 その後、このオブジェクトがクライアントの `Authorizer` プロパティで設定され、Azure へのクライアントのアクセスが許可されます。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

## <a name="use-file-based-authentication"></a>ファイル ベースの認証を使用する

ファイル ベースの認証は、[Azure CLI 2.0](/cli/azure) で生成されたローカル ファイル形式で保存されているクライアントの資格情報でのみ機能します。 `--sdk-auth` パラメーターを指定して新しいサービス プリンシパルを作成すると、このファイルを簡単に作成できます。 ファイル ベースの認証を使用する場合は、サービス プリンシパルの作成時に、この引数が指定されていることを確認してください。 CLI では出力が `stdout` に生成されるので、出力をファイルにリダイレクトします。

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

`AZURE_AUTH_LOCATION` 環境変数を、承認ファイルがある場所に設定します。 この環境変数をアプリケーションが読み取り、環境変数内の資格情報が解析されます。 承認ファイルを実行時に選択する必要がある場合は、[os.Setenv](https://golang.org/pkg/os/#Setenv) 関数を使用してプログラムの環境を操作します。

認証情報を読み込むには、[NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) 関数を呼び出します。 環境ベースの承認とは異なり、ファイル ベースの承認にはリソース エンドポイントが必要です。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

サービス プリンシパルの使用とアクセス許可の管理の詳細については、[Azure CLI 2.0 でのサービス プリンシパルの作成]に関する記事をご覧ください。

## <a name="use-device-token-authentication"></a>デバイス トークン認証を使用する

ユーザーが対話形式でログインできるようにする場合、この機能を提供する最良の方法は、デバイス トークン認証を使用することです。 この認証フローでは、Microsoft ログイン サイトに貼り付けるトークンをユーザーに渡し、ユーザーは Azure Active Directory (AAD) アカウントでログインします。 標準のユーザー名/パスワード認証とは異なり、この認証方法では多要素認証が有効になっているアカウントがサポートされます。

デバイス トークン認証を使用するには、[NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) 関数で [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) Authorizer を作成します。 作成されたオブジェクトで [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) を呼び出して認証プロセスを開始します。 デバイス認証フローでは、認証フロー全体が完了するまでプログラムの実行がブロックされます。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>認証クライアントを使用する

特定の種類の認証が必要であり、ユーザーの認証情報を読み込む処理をプログラムで実行する場合は、[auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) インターフェイスに準拠したクライアントを使用できます。 対話型プログラムが必要な場合、特別な構成ファイルを使用する場合、または別の認証方法を使用できない要件がある場合は、このインターフェイスを実装する型を使用します。

> [!WARNING]
> Azure の資格情報をアプリケーションにハードコードしないでください。 シークレットをアプリケーションのバイナリに配置すると、アプリケーションが実行されているかどうかに関係なく、攻撃者がシークレットを簡単に抽出できるようになります。 この場合、資格情報が承認されているすべての Azure リソースが危険にさらされます。

次の表に、`AuthorizerConfig` インターフェイスに準拠した SDK の型を示します。

| 認証の種類 | Authorizer の型 |
|---------------------|-----------------------|
| 証明書ベースの認証 | [ClientCertificateConfig] |
| クライアントの資格情報 | [ClientCredentialsConfig] |
| 管理対象サービス ID (MSI) | [MSIConfig] |
| ユーザー名/パスワード | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

関連付けられた `New` 関数で認証子を作成し、作成されたオブジェクトで `Authorize` を呼び出して認証を実行します。 たとえば、証明書ベースの認証を使用する場合は次のようになります。

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
