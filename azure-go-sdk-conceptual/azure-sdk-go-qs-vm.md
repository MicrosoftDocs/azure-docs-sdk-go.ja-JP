---
title: Go から Azure 仮想マシンをデプロイする
description: Azure SDK for Go を使用して仮想マシンをデプロイします。
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059137"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>クイック スタート: Azure SDK for Go を使用してテンプレートから Azure 仮想マシンをデプロイする

このクイック スタートでは、Azure SDK for Go を使用して Azure Resource Manager テンプレートからリソースをデプロイする方法を示します。 テンプレートは、[Azure リソース グループ](/azure/azure-resource-manager/resource-group-overview)内のすべてのリソースのスナップショットです。 この過程で、SDK の機能と規則について理解を深めます。

このクイック スタートの終わりには、実行中の VM にユーザー名とパスワードを使用してログインした状態になります。

> [!NOTE]
> Resource Manager テンプレートを使用せずに Go 内で VM を作成する方法を確認する場合は、この SDK を使用して VM のすべてのリソースを作成して構成する方法を説明した[命令型のサンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go)があります。 このサンプルのテンプレートを使用すると、Azure サービスのアーキテクチャに関する数多くの詳細に深入りすることなく、SDK の規則に集中できます。

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Azure CLI のローカル インストールを使用する場合、このクイック スタートには CLI バージョン __2.0.28__ 以降が必要です。 `az --version` を実行して、CLI インストールがこの要件を満たしていることを確認してください。 インストールまたはアップグレードが必要な場合は、[Azure CLI のインストール](/cli/azure/install-azure-cli)に関するページを参照してください。

## <a name="install-the-azure-sdk-for-go"></a>Azure SDK for Go のインストール

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>サービス プリンシパルの作成

アプリケーションで非対話形式で Azure にサインインするには、サービス プリンシパルが必要です。 サービス プリンシパルはロールベースのアクセス制御 (RBAC) の一部であり、これによって一意のユーザー ID が作成されます。 CLI で新しいサービス プリンシパルを作成するには、次のコマンドを実行します。

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

`AZURE_AUTH_LOCATION` 環境変数を、このファイルの完全なパスに設定します。 これにより、SDK によってこのファイルが検索され、ファイルから資格情報が直接読み取られます。サービス プリンシパルの情報を変更したり、記録したりする必要はありません。

## <a name="get-the-code"></a>コードの入手

`go get` を使用して、クイック スタート コードとすべての依存関係を入手します。

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

`AZURE_AUTH_LOCATION` 変数が適切に設定されていれば、ソース コードを変更する必要はありません。 プログラムが実行されると、必要なすべての認証情報がそこから読み込まれます。

## <a name="running-the-code"></a>コードの実行

`go run` コマンドを使用してクイック スタートを実行します。

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

デプロイが成功すると、新しく作成された仮想マシンにログインするためのユーザー名、IP アドレス、パスワードを示すメッセージが表示されます。 このマシンに SSH 接続し、マシンが起動して動作しているかどうかを確認します。 

## <a name="cleaning-up"></a>クリーンアップしています

このクイック スタートで作成したリソースをクリーンアップするには、CLI を使用してリソース グループを削除します。

```azurecli-interactive
az group delete -n GoVMQuickstart
```

作成されたサービス プリンシパルも削除します。 `quickstart.auth` ファイルに `clientId` の JSON キーがあります。 この値を `CLIENT_ID_VALUE` 環境変数にコピーし、次の Azure CLI コマンドを実行します。

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

ここに `quickstart.auth` の `CLIENT_ID_VALUE` の値を指定します。

> [!WARNING]
> このアプリケーションのサービス プリンシパルの削除に失敗すると、アプリケーションは Azure Active Directory テナントでアクティブなままになります。
> このサービス プリンシパルの名前とパスワードの両方が UUID として生成されますが、使用されないサービス プリンシパルと Azure Active Directory アプリケーションをすべて削除することで、適切なセキュリティ プラクティスに必ず従ってください。

## <a name="code-in-depth"></a>コードの詳細

クイック スタート コードの内容は、変数のブロックといくつかの小さな関数に分かれています。ここでは、それぞれについて説明します。

### <a name="variables-constants-and-types"></a>変数、定数、型

クイック スタートは自己完結型であるため、グローバル定数とグローバル変数が使用されます。

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

作成するリソースの名前を指定する値が宣言されています。 ここでは場所も指定されています。この場所を変更して、他のデータセンターでのデプロイの動作を確認できます。 すべてのデータセンターで、必要なリソースがすべて提供されるとは限りません。

`clientInfo` 型には、SDK でクライアントをセットアップし、VM のパスワードを設定するために、認証ファイルから読み込まれた情報が保持されます。

定数 `templateFile` と `parametersFile` は、デプロイに必要なファイルを参照しています。 `authorizer` は、認証のために Go SDK によって構成されます。`ctx` 変数は、ネットワーク操作の [Go コンテキスト](https://blog.golang.org/context)です。

### <a name="authentication-and-initialization"></a>認証と初期化

`init` 関数では認証が設定されます。 認証はこのクイック スタートのあらゆるものの前提条件となるため、初期化に含めるのが合理的です。 また、クライアントと VM を構成するために必要な情報が認証ファイルから読み込まれます。

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

まず、[auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) が呼び出され、`AZURE_AUTH_LOCATION` にあるファイルから認証情報が読み込まれます。 次に、`readJSON` 関数によってこのファイルが手動で読み込まれ (ここでは省略)、プログラムの残りの部分を実行するために必要な 2 つの値 (クライアントのサブスクリプション ID と、VM のパスワードにも使用されるサービス プリンシパルのシークレット) が取得されます。

> [!WARNING]
> このクイック スタートをシンプルに保つために、サービス プリンシパルのパスワードが再利用されます。 運用環境では、Azure リソースにアクセスするためのパスワードを__再利用しない__ように注意してください。

### <a name="flow-of-operations-in-main"></a>main() の操作のフロー

`main` は、操作のフローを示し、エラー チェックを実行するだけの単純な関数です。

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

コードで実行される手順は次のとおりです。

* デプロイ先のリソース グループを作成する (`createGroup`)
* このグループ内にデプロイを作成する (`createDeployment`)
* デプロイした VM のログイン情報を取得して表示する (`getLogin`)

### <a name="create-the-resource-group"></a>リソース グループの作成

`createGroup` 関数はリソース グループを作成します。 呼び出しフローと引数は、SDK でサービスとの対話を構造化する方法を示しています。

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Azure サービスとの対話の一般的なフローは次のとおりです。

* `service.New*Client()` メソッドを使用してクライアントを作成します。`*` は、対話する `service` のリソースの種類です。 この関数は、常にサブスクリプション ID を取得します。
* クライアントの承認方法を設定して、クライアントがリモート API と対話できるようにします。
* リモート API に対応するクライアントでメソッド呼び出しを行います。 通常、サービス クライアント メソッドでは、リソースの名前とメタデータ オブジェクトを取得します。

ここでは、型変換を実行するために [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 関数が使用されています。 SDK のメソッドのパラメーターは、ほとんどがポインターを取得するので、型変換を容易にするために便利なメソッドが用意されています。 便利なコンバーターとその動作の一覧については、[autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) モジュールのドキュメントをご覧ください。

`groupsClient.CreateOrUpdate` メソッドは、リソース グループを表すデータ型へのポインターを返します。 この種の直接的な戻り値は、同期する必要がある実行時間の短い操作を示しています。 次のセクションでは、実行時間の長い操作の例と、その操作と対話する方法を説明します。

### <a name="perform-the-deployment"></a>デプロイの実行

リソース グループが作成されたら、デプロイを実行します。 このコードは、ロジックのさまざまな部分を強調するために小さなセクションに分けられています。

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
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

デプロイ ファイルは `readJSON` によって読み込まれますが、その詳細はここでは省略されています。 この関数は、`*map[string]interface{}` を返します。これは、リソース デプロイの呼び出しに必要なメタデータの作成に使用される型です。 デプロイ パラメーターで VM のパスワードも手動で設定します。

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

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
        return
    }
```

このコードは、リソース グループを作成する場合と同じパターンに従っています。 Azure で認証できることを前提として、新しいクライアントが作成された後、メソッドが呼び出されます。
このメソッドは、リソース グループの対応するメソッドと同じ名前 (`CreateOrUpdate`) です。 このパターンは SDK 全体で見られます。
同様の処理を実行するメソッドは、通常、同じ名前です。

最も大きな違いは、`deploymentsClient.CreateOrUpdate` メソッドの戻り値にあります。 この値は、[Future 設計パターン](https://en.wikipedia.org/wiki/Futures_and_promises)に従う [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) 型です。 Future は、ポーリング、取り消し、または完了のブロックが可能な、Azure での実行時間の長い操作を表します。

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

この例では、一番良いのは操作が完了するまで待つことです。 将来のある時点まで待つには、[コンテキスト オブジェクト](https://blog.golang.org/context)と、`Future` オブジェクトを作成したクライアントの両方が必要です。 ここではエラー ソースとして、メソッドを呼び出そうとしたときにクライアント側で発生したエラーと、サーバーからのエラー応答の 2 つが考えられます。 後者は、`deploymentFuture.Result` 呼び出しの一部として返されます。

### <a name="get-the-assigned-ip-address"></a>割り当てられた IP アドレスの取得

新しく作成された VM で 操作を実行するには、割り当てられた IP アドレスが必要です。 IP アドレスは、ネットワーク インターフェイス コントローラー (NIC) リソースにバインドされた個別の Azure リソースです。

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

このメソッドは、パラメーター ファイルに保存されている情報に依存します。 コードで VM を直接照会して NIC を取得し、NIC を照会して IP リソースを取得してから、IP リソースを直接照会することもできます。 この場合、依存関係と解決操作の長いチェーンになるため、コストがかかります。 JSON 情報はローカルであるため、代わりにこの情報を読み込むことができます。

VM ユーザーの値も JSON から読み込まれます。 VM のパスワードは、認証ファイルから既に読み込まれています。

## <a name="next-steps"></a>次の手順

このクイック スタートでは、既存のテンプレートを取得し、Go を使用してデプロイしました。 次に、新しく作成された VM に SSH 経由で接続しました。

Go を使用して Azure 環境の仮想マシンを操作する方法について引き続き学習する場合は、[Go 用 Azure コンピューティング サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)または [Go 用 Azure リソース管理サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)を参照してください。

SDK で使用可能な認証方法と、各認証方法でサポートされる認証の種類の詳細については、[Azure SDK for Go での認証](azure-sdk-go-authorization.md)に関する記事をご覧ください。
