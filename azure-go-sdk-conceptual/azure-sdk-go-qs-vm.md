---
title: Go から Azure 仮想マシンをデプロイする
description: Azure SDK for Go を使用して仮想マシンをデプロイします。
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 46a1243ff2ff6bfcf3831e2cea3137c1f6051c78
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>クイック スタート: Azure SDK for Go を使用してテンプレートから Azure 仮想マシンをデプロイする

このクイック スタートでは、Azure SDK for Go を使用してテンプレートからリソースをデプロイする方法について説明します。 テンプレートは、[Azure リソース グループ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)に含まれているすべてのリソースのスナップショットです。 途中で有用なタスクを実行しながら、SDK の機能と規則について理解を深めます。

このクイック スタートの終わりには、実行中の VM にユーザー名とパスワードを使用してログインした状態になります。

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Azure CLI のローカル インストールを使用する場合、このクイック スタートには CLI バージョン 2.0.24 以降が必要です。 `az --version` を実行して、CLI インストールがこの要件を満たしていることを確認してください。 インストールまたはアップグレードが必要な場合は、[Azure CLI 2.0 のインストール](/cli/azure/install-azure-cli)に関する記事をご覧ください。

## <a name="install-the-azure-sdk-for-go"></a>Azure SDK for Go のインストール 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>サービス プリンシパルの作成

アプリケーションで非対話形式のログインを行うには、サービス プリンシパルが必要です。 サービス プリンシパルはロールベースのアクセス制御 (RBAC) の一部であり、これによって一意のユーザー ID が作成されます。 CLI で新しいサービス プリンシパルを作成するには、次のコマンドを実行します。

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

出力の `appId`、`password`、`tenant` の値を__必ず__記録しておいてください。 これらの値は、アプリケーションが Azure での認証に使用します。

Azure CLI 2.0 でのサービス プリンシパルの作成と管理の詳細については、「[Azure CLI 2.0 で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)」をご覧ください。

## <a name="get-the-code"></a>コードの入手

`go get` を使用して、クイック スタート コードとすべての依存関係を入手します。

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

このコードはコンパイルされますが、Azure アカウントと作成したサービス プリンシパルに関する情報を指定するまで正しく実行されません。 `main.go` には、`authInfo` 構造体を格納する `config` 変数があります。 正しく認証するために、この構造体の各フィールド値を置き換える必要があります。

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: サブスクリプション ID。CLI コマンドを使用して取得できます。

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: テナント ID。サービス プリンシパルの作成時に記録した `tenant` 値。
* `ServicePrincipalID`: サービス プリンシパルの作成時に記録した `appId` 値。
* `ServicePrincipalSecret`: サービス プリンシパルの作成時に記録した `password` 値。

`vm-quickstart-params.json` ファイル内の値も編集する必要があります。

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: VM ユーザー アカウントのパスワード。 長さが 12 ～ 72 文字で、次のうち 3 種類の文字を含める必要があります。
  * 小文字
  * 大文字
  * 数字
  * 記号

## <a name="running-the-code"></a>コードの実行

`go run` コマンドを使用してクイック スタートを実行します。

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

デプロイでエラーが発生すると、問題があることを示すメッセージが表示されますが、詳細は含まれていません。 デプロイ エラーの詳細を取得するには、Azure CLI で次のコマンドを実行します。

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

デプロイが成功すると、新しく作成された仮想マシンにログインするためのユーザー名、IP アドレス、パスワードを示すメッセージが表示されます。 このマシンに SSH 接続し、マシンが起動して動作していることを確認します。

## <a name="cleaning-up"></a>クリーンアップしています

このクイック スタートで作成したリソースをクリーンアップするには、CLI を使用してリソース グループを削除します。

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>コードの詳細

クイック スタート コードの内容は、変数のブロックといくつかの小さな関数に分かれています。ここでは、それぞれについて説明します。

### <a name="variable-assignments-and-structs"></a>変数代入と構造体

クイック スタートは自己完結型であるため、コマンド ライン オプションや環境変数ではなく、グローバル変数を使用しています。

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

Azure サービスによる承認に必要なすべての情報をカプセル化するために、`authInfo` 構造体が宣言されています。

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

作成するリソースの名前を指定する値が宣言されています。 ここでは場所も指定されています。この場所を変更して、他のデータセンターでのデプロイの動作を確認できます。 すべてのデータセンターで、必要なリソースがすべて提供されるとは限りません。

定数 `templateFile` と `parametersFile` は、デプロイに必要なファイルを参照しています。 サービス プリンシパル トークンについては後述します。`ctx` 変数は、ネットワーク操作の [Go コンテキスト](https://blog.golang.org/context)です。

### <a name="init-and-authorization"></a>init() と承認

コードの `init()` メソッドでは承認を設定します。 承認は、このクイック スタートのあらゆるものの前提条件となるため、初期化に含めるのが合理的です。 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

このコードでは、承認のために次の 2 つの手順を実行します。

* Azure Active Directory と連動して、`TenantID` の OAuth 構成情報が取得されます。 [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) オブジェクトには、標準の Azure 構成で使用されるエンドポイントが含まれています。
* [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) 関数が呼び出されます。 この関数は、サービス プリンシパルのログインと共に OAuth 情報を取得し、使用されている Azure 管理のスタイルの情報も取得します。 特定の要件があり、実行内容を把握している場合を除き、この値には必ず `.ResourceManagerEndpoint` を指定してください。

### <a name="flow-of-operations-in-main"></a>main() の操作のフロー

`main()` は、操作のフローを示し、エラー チェックを実行するだけの単純な関数です。

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

コードで実行される手順は次のとおりです。

* デプロイ先のリソース グループを作成する (`createGroup()`)
* このグループ内にデプロイを作成する (`createDeployment()`)
* デプロイした VM のログイン情報を取得して表示する (`getLogin()`)

### <a name="creating-the-resource-group"></a>リソース グループの作成

`createGroup()` 関数はリソース グループを作成します。 呼び出しフローと引数は、SDK でサービスとの対話を構造化する方法を示しています。

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Azure サービスとの対話の一般的なフローは次のとおりです。

* `service.NewXClient()` メソッドを使用してクライアントを作成します。`X` は、対話する `service` のリソースの種類です。 この関数は、常にサブスクリプション ID を取得します。
* クライアントの承認方法を設定して、クライアントがリモート API と対話できるようにします。
* リモート API に対応するクライアントでメソッド呼び出しを行います。 通常、サービス クライアント メソッドでは、リソースの名前とメタデータ オブジェクトを取得します。

ここでは、型変換を実行するために [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 関数が使用されています。 SDK のメソッドのパラメーター構造体は、ほとんどがポインターを取得するので、型変換を容易にするためにこれらのメソッドが用意されています。 便利なコンバーターの一覧と動作については、[autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) モジュールのドキュメントをご覧ください。

`groupsClient.CreateOrUpdate()` 操作は、リソース グループを表すデータ構造体へのポインターを返します。 この種の直接的な戻り値は、同期する必要がある実行時間の短い操作を示しています。 次のセクションでは、実行時間の長い操作の例とそれらの操作と対話する方法を確認します。

### <a name="performing-the-deployment"></a>デプロイの実行

リソースを含むグループが作成されたら、デプロイを実行します。 このコードは、ロジックのさまざまな部分を強調するために小さなセクションに分けられています。


```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }

        // ...
```

デプロイ ファイルは `readJSON` によって読み込まれますが、その詳細はここでは省略されています。 この関数は、`*map[string]interface{}` を返します。これは、リソース デプロイの呼び出しに必要なメタデータの作成に使用される型です。

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        deploymentFuture, err := deploymentsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                deploymentName,
                resources.Deployment{
                        Properties: &resources.DeploymentProperties{
                                Template:   template,
                                Parameters: params,
                                Mode:       resources.Incremental,
                        },
                },
        )
        if err != nil {
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

このコードは、リソース グループを作成する場合と同じパターンに従っています。 Azure で認証できることを前提として、新しいクライアントが作成された後、メソッドが呼び出されます。 このメソッドは、リソース グループの対応するメソッドと同じ名前 (`CreateOrUpdate`) です。 このパターンは SDK で何度も見られます。 同様の処理を実行するメソッドは、通常、同じ名前です。

最も大きな違いは、`deploymentsClient.CreateOrUpdate()` メソッドの戻り値にあります。 この値は、[Future 設計パターン](https://en.wikipedia.org/wiki/Futures_and_promises)に従う `Future` オブジェクトです。 Future は、Azure で長時間実行され、他の処理の実行中に場合によってはポーリングできる操作を表します。

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

この例では、一番良いのは操作が完了するまで待つことです。 将来のある時点まで待つには、[コンテキスト オブジェクト](https://blog.golang.org/context)と、Future オブジェクトを作成したクライアントの両方が必要です。 ここではエラー ソースとして、メソッドを呼び出そうとしたときにクライアント側で発生したエラーと、サーバーからのエラー応答の 2 つが考えられます。 後者は、`deploymentFuture.Result()` 呼び出しの一部として返されます。

### <a name="obtaining-the-assigned-ip-address"></a>割り当てられた IP アドレスの取得

新しく作成された VM で 操作を実行するには、割り当てられた IP アドレスが必要です。 IP アドレスは、ネットワーク インターフェイス コントローラー (NIC) リソースにバインドされた個別の Azure リソースです。

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

このメソッドは、パラメーター ファイルに保存されている情報に依存します。 コードで VM を直接照会して NIC を取得し、NIC を照会して IP リソースを取得してから、IP リソースを直接照会することもできます。 この場合、依存関係と解決操作の長いチェーンになるため、コストがかかります。 JSON 情報はローカルであるため、代わりにこの情報を読み込むことができます。

VM ユーザーとパスワードの値も、JSON から同様に読み込まれます。

## <a name="next-steps"></a>次の手順

このクイック スタートでは、既存のテンプレートを取得し、Go を使用してデプロイしました。 次に、新しく作成された VM に SSH 経由で接続して、VM が実行されていることを確認しました。

Go を使用して Azure 環境の仮想マシンを操作する方法について引き続き学習する場合は、[Go 用 Azure コンピューティング サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)または [Go 用 Azure リソース管理サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)を参照してください。
