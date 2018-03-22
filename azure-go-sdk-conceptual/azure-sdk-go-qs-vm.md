---
title: "Go から Azure 仮想マシンをデプロイする"
description: "Azure SDK for Go を使用して仮想マシンをデプロイします。"
keywords: "azure, 仮想マシン, vm, go, golang, azure sdk"
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="cc286-104">クイック スタート: Azure SDK for Go を使用してテンプレートから Azure 仮想マシンをデプロイする</span><span class="sxs-lookup"><span data-stu-id="cc286-104">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="cc286-105">このクイック スタートでは、Azure SDK for Go を使用してテンプレートからリソースをデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="cc286-105">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="cc286-106">テンプレートは、[Azure リソース グループ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)に含まれているすべてのリソースのスナップショットです。</span><span class="sxs-lookup"><span data-stu-id="cc286-106">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="cc286-107">途中で有用なタスクを実行しながら、SDK の機能と規則について理解を深めます。</span><span class="sxs-lookup"><span data-stu-id="cc286-107">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="cc286-108">このクイック スタートの終わりには、実行中の VM にユーザー名とパスワードを使用してログインした状態になります。</span><span class="sxs-lookup"><span data-stu-id="cc286-108">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="cc286-109">Azure CLI のローカル インストールを使用する場合、このクイック スタートには CLI バージョン 2.0.24 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="cc286-109">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="cc286-110">`az --version` を実行して、CLI インストールがこの要件を満たしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="cc286-110">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="cc286-111">インストールまたはアップグレードが必要な場合は、[Azure CLI 2.0 のインストール](/cli/azure/install-azure-cli)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cc286-111">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="cc286-112">Azure SDK for Go のインストール</span><span class="sxs-lookup"><span data-stu-id="cc286-112">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="cc286-113">サービス プリンシパルの作成</span><span class="sxs-lookup"><span data-stu-id="cc286-113">Create a service principal</span></span>

<span data-ttu-id="cc286-114">アプリケーションで非対話形式のログインを行うには、サービス プリンシパルが必要です。</span><span class="sxs-lookup"><span data-stu-id="cc286-114">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="cc286-115">サービス プリンシパルはロールベースのアクセス制御 (RBAC) の一部であり、これによって一意のユーザー ID が作成されます。</span><span class="sxs-lookup"><span data-stu-id="cc286-115">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="cc286-116">CLI で新しいサービス プリンシパルを作成するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc286-116">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="cc286-117">出力の `appId`、`password`、`tenant` の値を__必ず__記録しておいてください。</span><span class="sxs-lookup"><span data-stu-id="cc286-117">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="cc286-118">これらの値は、アプリケーションが Azure での認証に使用します。</span><span class="sxs-lookup"><span data-stu-id="cc286-118">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="cc286-119">Azure CLI 2.0 でのサービス プリンシパルの作成と管理の詳細については、「[Azure CLI 2.0 で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cc286-119">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="cc286-120">コードの入手</span><span class="sxs-lookup"><span data-stu-id="cc286-120">Get the code</span></span>

<span data-ttu-id="cc286-121">`go get` を使用して、クイック スタート コードとすべての依存関係を入手します。</span><span class="sxs-lookup"><span data-stu-id="cc286-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="cc286-122">このコードはコンパイルされますが、Azure アカウントと作成したサービス プリンシパルに関する情報を指定するまで正しく実行されません。</span><span class="sxs-lookup"><span data-stu-id="cc286-122">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="cc286-123">`main.go` には、`authInfo` 構造体を格納する `config` 変数があります。</span><span class="sxs-lookup"><span data-stu-id="cc286-123">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="cc286-124">正しく認証するために、この構造体の各フィールド値を置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc286-124">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="cc286-125">`SubscriptionID`: サブスクリプション ID。CLI コマンドを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="cc286-125">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="cc286-126">`TenantID`: テナント ID。サービス プリンシパルの作成時に記録した `tenant` 値。</span><span class="sxs-lookup"><span data-stu-id="cc286-126">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="cc286-127">`ServicePrincipalID`: サービス プリンシパルの作成時に記録した `appId` 値。</span><span class="sxs-lookup"><span data-stu-id="cc286-127">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="cc286-128">`ServicePrincipalSecret`: サービス プリンシパルの作成時に記録した `password` 値。</span><span class="sxs-lookup"><span data-stu-id="cc286-128">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="cc286-129">`vm-quickstart-params.json` ファイル内の値も編集する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc286-129">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="cc286-130">`vm_password`: VM ユーザー アカウントのパスワード。</span><span class="sxs-lookup"><span data-stu-id="cc286-130">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="cc286-131">長さが 12 ～ 72 文字で、次のうち 3 種類の文字を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc286-131">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="cc286-132">小文字</span><span class="sxs-lookup"><span data-stu-id="cc286-132">A lowercase letter</span></span>
  * <span data-ttu-id="cc286-133">大文字</span><span class="sxs-lookup"><span data-stu-id="cc286-133">An uppercase letter</span></span>
  * <span data-ttu-id="cc286-134">数字</span><span class="sxs-lookup"><span data-stu-id="cc286-134">A number</span></span>
  * <span data-ttu-id="cc286-135">記号</span><span class="sxs-lookup"><span data-stu-id="cc286-135">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="cc286-136">コードの実行</span><span class="sxs-lookup"><span data-stu-id="cc286-136">Running the code</span></span>

<span data-ttu-id="cc286-137">`go run` コマンドを使用してクイック スタートを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc286-137">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="cc286-138">デプロイでエラーが発生すると、問題があることを示すメッセージが表示されますが、詳細は含まれていません。</span><span class="sxs-lookup"><span data-stu-id="cc286-138">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="cc286-139">デプロイ エラーの詳細を取得するには、Azure CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc286-139">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="cc286-140">デプロイが成功すると、新しく作成された仮想マシンにログインするためのユーザー名、IP アドレス、パスワードを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc286-140">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="cc286-141">このマシンに SSH 接続し、マシンが起動して動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cc286-141">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="cc286-142">クリーンアップしています</span><span class="sxs-lookup"><span data-stu-id="cc286-142">Cleaning up</span></span>

<span data-ttu-id="cc286-143">このクイック スタートで作成したリソースをクリーンアップするには、CLI を使用してリソース グループを削除します。</span><span class="sxs-lookup"><span data-stu-id="cc286-143">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="cc286-144">コードの詳細</span><span class="sxs-lookup"><span data-stu-id="cc286-144">Code in depth</span></span>

<span data-ttu-id="cc286-145">クイック スタート コードの内容は、変数のブロックといくつかの小さな関数に分かれています。ここでは、それぞれについて説明します。</span><span class="sxs-lookup"><span data-stu-id="cc286-145">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="cc286-146">変数代入と構造体</span><span class="sxs-lookup"><span data-stu-id="cc286-146">Variable assignments and structs</span></span>

<span data-ttu-id="cc286-147">クイック スタートは自己完結型であるため、コマンド ライン オプションや環境変数ではなく、グローバル変数を使用しています。</span><span class="sxs-lookup"><span data-stu-id="cc286-147">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="cc286-148">Azure サービスによる承認に必要なすべての情報をカプセル化するために、`authInfo` 構造体が宣言されています。</span><span class="sxs-lookup"><span data-stu-id="cc286-148">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="cc286-149">作成するリソースの名前を指定する値が宣言されています。</span><span class="sxs-lookup"><span data-stu-id="cc286-149">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="cc286-150">ここでは場所も指定されています。この場所を変更して、他のデータセンターでのデプロイの動作を確認できます。</span><span class="sxs-lookup"><span data-stu-id="cc286-150">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="cc286-151">すべてのデータセンターで、必要なリソースがすべて提供されるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="cc286-151">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="cc286-152">定数 `templateFile` と `parametersFile` は、デプロイに必要なファイルを参照しています。</span><span class="sxs-lookup"><span data-stu-id="cc286-152">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="cc286-153">サービス プリンシパル トークンについては後述します。`ctx` 変数は、ネットワーク操作の [Go コンテキスト](https://blog.golang.org/context)です。</span><span class="sxs-lookup"><span data-stu-id="cc286-153">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="cc286-154">init() と承認</span><span class="sxs-lookup"><span data-stu-id="cc286-154">init() and authorization</span></span>

<span data-ttu-id="cc286-155">コードの `init()` メソッドでは承認を設定します。</span><span class="sxs-lookup"><span data-stu-id="cc286-155">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="cc286-156">承認は、このクイック スタートのあらゆるものの前提条件となるため、初期化に含めるのが合理的です。</span><span class="sxs-lookup"><span data-stu-id="cc286-156">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="cc286-157">このコードでは、承認のために次の 2 つの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="cc286-157">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="cc286-158">Azure Active Directory と連動して、`TenantID` の OAuth 構成情報が取得されます。</span><span class="sxs-lookup"><span data-stu-id="cc286-158">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="cc286-159">[`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) オブジェクトには、標準の Azure 構成で使用されるエンドポイントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cc286-159">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="cc286-160">[`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) 関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="cc286-160">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="cc286-161">この関数は、サービス プリンシパルのログインと共に OAuth 情報を取得し、使用されている Azure 管理のスタイルの情報も取得します。</span><span class="sxs-lookup"><span data-stu-id="cc286-161">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="cc286-162">特定の要件があり、実行内容を把握している場合を除き、この値には必ず `.ResourceManagerEndpoint` を指定してください。</span><span class="sxs-lookup"><span data-stu-id="cc286-162">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="cc286-163">main() の操作のフロー</span><span class="sxs-lookup"><span data-stu-id="cc286-163">Flow of operations in main()</span></span>

<span data-ttu-id="cc286-164">`main()` は、操作のフローを示し、エラー チェックを実行するだけの単純な関数です。</span><span class="sxs-lookup"><span data-stu-id="cc286-164">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="cc286-165">コードで実行される手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="cc286-165">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="cc286-166">デプロイ先のリソース グループを作成する (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="cc286-166">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="cc286-167">このグループ内にデプロイを作成する (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="cc286-167">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="cc286-168">デプロイした VM のログイン情報を取得して表示する (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="cc286-168">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="cc286-169">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="cc286-169">Creating the resource group</span></span>

<span data-ttu-id="cc286-170">`createGroup()` 関数はリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="cc286-170">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="cc286-171">呼び出しフローと引数は、SDK でサービスとの対話を構造化する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="cc286-171">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="cc286-172">Azure サービスとの対話の一般的なフローは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="cc286-172">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="cc286-173">`service.NewXClient()` メソッドを使用してクライアントを作成します。`X` は、対話する `service` のリソースの種類です。</span><span class="sxs-lookup"><span data-stu-id="cc286-173">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="cc286-174">この関数は、常にサブスクリプション ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="cc286-174">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="cc286-175">クライアントの承認方法を設定して、クライアントがリモート API と対話できるようにします。</span><span class="sxs-lookup"><span data-stu-id="cc286-175">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="cc286-176">リモート API に対応するクライアントでメソッド呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="cc286-176">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="cc286-177">通常、サービス クライアント メソッドでは、リソースの名前とメタデータ オブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="cc286-177">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="cc286-178">ここでは、型変換を実行するために [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 関数が使用されています。</span><span class="sxs-lookup"><span data-stu-id="cc286-178">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="cc286-179">SDK のメソッドのパラメーター構造体は、ほとんどがポインターを取得するので、型変換を容易にするためにこれらのメソッドが用意されています。</span><span class="sxs-lookup"><span data-stu-id="cc286-179">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="cc286-180">便利なコンバーターの一覧と動作については、[autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) モジュールのドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cc286-180">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="cc286-181">`groupsClient.CreateOrUpdate()` 操作は、リソース グループを表すデータ構造体へのポインターを返します。</span><span class="sxs-lookup"><span data-stu-id="cc286-181">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="cc286-182">この種の直接的な戻り値は、同期する必要がある実行時間の短い操作を示しています。</span><span class="sxs-lookup"><span data-stu-id="cc286-182">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="cc286-183">次のセクションでは、実行時間の長い操作の例とそれらの操作と対話する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="cc286-183">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="cc286-184">デプロイの実行</span><span class="sxs-lookup"><span data-stu-id="cc286-184">Performing the deployment</span></span>

<span data-ttu-id="cc286-185">リソースを含むグループが作成されたら、デプロイを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc286-185">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="cc286-186">このコードは、ロジックのさまざまな部分を強調するために小さなセクションに分けられています。</span><span class="sxs-lookup"><span data-stu-id="cc286-186">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="cc286-187">デプロイ ファイルは `readJSON` によって読み込まれますが、その詳細はここでは省略されています。</span><span class="sxs-lookup"><span data-stu-id="cc286-187">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="cc286-188">この関数は、`*map[string]interface{}` を返します。これは、リソース デプロイの呼び出しに必要なメタデータの作成に使用される型です。</span><span class="sxs-lookup"><span data-stu-id="cc286-188">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="cc286-189">このコードは、リソース グループを作成する場合と同じパターンに従っています。</span><span class="sxs-lookup"><span data-stu-id="cc286-189">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="cc286-190">Azure で認証できることを前提として、新しいクライアントが作成された後、メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="cc286-190">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="cc286-191">このメソッドは、リソース グループの対応するメソッドと同じ名前 (`CreateOrUpdate`) です。</span><span class="sxs-lookup"><span data-stu-id="cc286-191">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="cc286-192">このパターンは SDK で何度も見られます。</span><span class="sxs-lookup"><span data-stu-id="cc286-192">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="cc286-193">同様の処理を実行するメソッドは、通常、同じ名前です。</span><span class="sxs-lookup"><span data-stu-id="cc286-193">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="cc286-194">最も大きな違いは、`deploymentsClient.CreateOrUpdate()` メソッドの戻り値にあります。</span><span class="sxs-lookup"><span data-stu-id="cc286-194">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="cc286-195">この値は、[Future 設計パターン](https://en.wikipedia.org/wiki/Futures_and_promises)に従う `Future` オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="cc286-195">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="cc286-196">Future は、Azure で長時間実行され、他の処理の実行中に場合によってはポーリングできる操作を表します。</span><span class="sxs-lookup"><span data-stu-id="cc286-196">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="cc286-197">この例では、一番良いのは操作が完了するまで待つことです。</span><span class="sxs-lookup"><span data-stu-id="cc286-197">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="cc286-198">将来のある時点まで待つには、[コンテキスト オブジェクト](https://blog.golang.org/context)と、Future オブジェクトを作成したクライアントの両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="cc286-198">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="cc286-199">ここではエラー ソースとして、メソッドを呼び出そうとしたときにクライアント側で発生したエラーと、サーバーからのエラー応答の 2 つが考えられます。</span><span class="sxs-lookup"><span data-stu-id="cc286-199">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="cc286-200">後者は、`deploymentFuture.Result()` 呼び出しの一部として返されます。</span><span class="sxs-lookup"><span data-stu-id="cc286-200">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="cc286-201">割り当てられた IP アドレスの取得</span><span class="sxs-lookup"><span data-stu-id="cc286-201">Obtaining the assigned IP address</span></span>

<span data-ttu-id="cc286-202">新しく作成された VM で 操作を実行するには、割り当てられた IP アドレスが必要です。</span><span class="sxs-lookup"><span data-stu-id="cc286-202">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="cc286-203">IP アドレスは、ネットワーク インターフェイス コントローラー (NIC) リソースにバインドされた個別の Azure リソースです。</span><span class="sxs-lookup"><span data-stu-id="cc286-203">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="cc286-204">このメソッドは、パラメーター ファイルに保存されている情報に依存します。</span><span class="sxs-lookup"><span data-stu-id="cc286-204">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="cc286-205">コードで VM を直接照会して NIC を取得し、NIC を照会して IP リソースを取得してから、IP リソースを直接照会することもできます。</span><span class="sxs-lookup"><span data-stu-id="cc286-205">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="cc286-206">この場合、依存関係と解決操作の長いチェーンになるため、コストがかかります。</span><span class="sxs-lookup"><span data-stu-id="cc286-206">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="cc286-207">JSON 情報はローカルであるため、代わりにこの情報を読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="cc286-207">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="cc286-208">VM ユーザーとパスワードの値も、JSON から同様に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="cc286-208">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc286-209">次の手順</span><span class="sxs-lookup"><span data-stu-id="cc286-209">Next steps</span></span>

<span data-ttu-id="cc286-210">このクイック スタートでは、既存のテンプレートを取得し、Go を使用してデプロイしました。</span><span class="sxs-lookup"><span data-stu-id="cc286-210">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="cc286-211">次に、新しく作成された VM に SSH 経由で接続して、VM が実行されていることを確認しました。</span><span class="sxs-lookup"><span data-stu-id="cc286-211">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="cc286-212">Go を使用して Azure 環境の仮想マシンを操作する方法について引き続き学習する場合は、[Go 用 Azure コンピューティング サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)または [Go 用 Azure リソース管理サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cc286-212">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
