---
title: Go から Azure 仮想マシンをデプロイする
description: Azure SDK for Go を使用して仮想マシンをデプロイします。
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="9938c-103">クイック スタート: Azure SDK for Go を使用してテンプレートから Azure 仮想マシンをデプロイする</span><span class="sxs-lookup"><span data-stu-id="9938c-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="9938c-104">このクイック スタートでは、Azure SDK for Go を使用してテンプレートからリソースをデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9938c-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="9938c-105">テンプレートは、[Azure リソース グループ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)に含まれているすべてのリソースのスナップショットです。</span><span class="sxs-lookup"><span data-stu-id="9938c-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="9938c-106">途中で有用なタスクを実行しながら、SDK の機能と規則について理解を深めます。</span><span class="sxs-lookup"><span data-stu-id="9938c-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="9938c-107">このクイック スタートの終わりには、実行中の VM にユーザー名とパスワードを使用してログインした状態になります。</span><span class="sxs-lookup"><span data-stu-id="9938c-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="9938c-108">Azure CLI のローカル インストールを使用する場合、このクイック スタートには CLI バージョン __2.0.28__ 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="9938c-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="9938c-109">`az --version` を実行して、CLI インストールがこの要件を満たしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="9938c-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="9938c-110">インストールまたはアップグレードが必要な場合は、[Azure CLI 2.0 のインストール](/cli/azure/install-azure-cli)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9938c-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="9938c-111">Azure SDK for Go のインストール</span><span class="sxs-lookup"><span data-stu-id="9938c-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="9938c-112">サービス プリンシパルの作成</span><span class="sxs-lookup"><span data-stu-id="9938c-112">Create a service principal</span></span>


<span data-ttu-id="9938c-113">アプリケーションで非対話形式のログインを行うには、サービス プリンシパルが必要です。</span><span class="sxs-lookup"><span data-stu-id="9938c-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="9938c-114">サービス プリンシパルはロールベースのアクセス制御 (RBAC) の一部であり、これによって一意のユーザー ID が作成されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="9938c-115">CLI で新しいサービス プリンシパルを作成するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="9938c-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="9938c-116">`AZURE_AUTH_LOCATION` 環境変数を、このファイルの完全なパスに設定します。</span><span class="sxs-lookup"><span data-stu-id="9938c-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="9938c-117">これにより、SDK によってこのファイルが検索され、ファイルから資格情報が直接読み取られます。サービス プリンシパルの情報を変更したり、記録したりする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9938c-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="9938c-118">コードの入手</span><span class="sxs-lookup"><span data-stu-id="9938c-118">Get the code</span></span>

<span data-ttu-id="9938c-119">`go get` を使用して、クイック スタート コードとすべての依存関係を入手します。</span><span class="sxs-lookup"><span data-stu-id="9938c-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="9938c-120">`AZURE_AUTH_LOCATION` 変数が適切に設定されていれば、ソース コードを変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9938c-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="9938c-121">プログラムが実行されると、必要なすべての認証情報がそこから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="9938c-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="9938c-122">コードの実行</span><span class="sxs-lookup"><span data-stu-id="9938c-122">Running the code</span></span>

<span data-ttu-id="9938c-123">`go run` コマンドを使用してクイック スタートを実行します。</span><span class="sxs-lookup"><span data-stu-id="9938c-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="9938c-124">デプロイでエラーが発生すると、問題があることを示すメッセージが表示されますが、十分な詳細が含まれていない場合があります。</span><span class="sxs-lookup"><span data-stu-id="9938c-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="9938c-125">デプロイ エラーの全詳細を取得するには、Azure CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="9938c-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="9938c-126">デプロイが成功すると、新しく作成された仮想マシンにログインするためのユーザー名、IP アドレス、パスワードを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="9938c-127">このマシンに SSH 接続し、マシンが起動して動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9938c-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="9938c-128">クリーンアップしています</span><span class="sxs-lookup"><span data-stu-id="9938c-128">Cleaning up</span></span>

<span data-ttu-id="9938c-129">このクイック スタートで作成したリソースをクリーンアップするには、CLI を使用してリソース グループを削除します。</span><span class="sxs-lookup"><span data-stu-id="9938c-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="9938c-130">コードの詳細</span><span class="sxs-lookup"><span data-stu-id="9938c-130">Code in depth</span></span>

<span data-ttu-id="9938c-131">クイック スタート コードの内容は、変数のブロックといくつかの小さな関数に分かれています。ここでは、それぞれについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9938c-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="9938c-132">変数、定数、型</span><span class="sxs-lookup"><span data-stu-id="9938c-132">Variables, constants, and types</span></span>

<span data-ttu-id="9938c-133">クイック スタートは自己完結型であるため、グローバル定数とグローバル変数が使用されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="9938c-134">作成するリソースの名前を指定する値が宣言されています。</span><span class="sxs-lookup"><span data-stu-id="9938c-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="9938c-135">ここでは場所も指定されています。この場所を変更して、他のデータセンターでのデプロイの動作を確認できます。</span><span class="sxs-lookup"><span data-stu-id="9938c-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="9938c-136">すべてのデータセンターで、必要なリソースがすべて提供されるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="9938c-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="9938c-137">SDK でクライアントをセットアップし、VM のパスワードを設定するために認証ファイルから個別に読み込む必要があるすべての情報をカプセル化するために、`clientInfo` 型が宣言されています。</span><span class="sxs-lookup"><span data-stu-id="9938c-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="9938c-138">定数 `templateFile` と `parametersFile` は、デプロイに必要なファイルを参照しています。</span><span class="sxs-lookup"><span data-stu-id="9938c-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="9938c-139">`authorizer` は、認証のために Go SDK によって構成されます。`ctx` 変数は、ネットワーク操作の [Go コンテキスト](https://blog.golang.org/context)です。</span><span class="sxs-lookup"><span data-stu-id="9938c-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="9938c-140">認証と初期化</span><span class="sxs-lookup"><span data-stu-id="9938c-140">Authentication and initialization</span></span>

<span data-ttu-id="9938c-141">`init` 関数では認証が設定されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="9938c-142">認証はこのクイック スタートのあらゆるものの前提条件となるため、初期化に含めるのが合理的です。</span><span class="sxs-lookup"><span data-stu-id="9938c-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="9938c-143">また、クライアントと VM を構成するために必要な情報が認証ファイルから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="9938c-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="9938c-144">まず、[auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) が呼び出され、`AZURE_AUTH_LOCATION` にあるファイルから認証情報が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="9938c-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="9938c-145">次に、`readJSON` 関数によってこのファイルが手動で読み込まれ (ここでは省略)、プログラムの残りの部分を実行するために必要な 2 つの値 (クライアントのサブスクリプション ID と、VM のパスワードにも使用されるサービス プリンシパルのシークレット) が取得されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="9938c-146">このクイック スタートをシンプルに保つために、サービス プリンシパルのパスワードが再利用されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="9938c-147">運用環境では、Azure リソースにアクセスするためのパスワードを__再利用しない__ように注意してください。</span><span class="sxs-lookup"><span data-stu-id="9938c-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="9938c-148">main() の操作のフロー</span><span class="sxs-lookup"><span data-stu-id="9938c-148">Flow of operations in main()</span></span>

<span data-ttu-id="9938c-149">`main` は、操作のフローを示し、エラー チェックを実行するだけの単純な関数です。</span><span class="sxs-lookup"><span data-stu-id="9938c-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="9938c-150">コードで実行される手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="9938c-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="9938c-151">デプロイ先のリソース グループを作成する (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="9938c-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="9938c-152">このグループ内にデプロイを作成する (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="9938c-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="9938c-153">デプロイした VM のログイン情報を取得して表示する (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="9938c-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="9938c-154">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="9938c-154">Creating the resource group</span></span>

<span data-ttu-id="9938c-155">`createGroup` 関数はリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="9938c-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="9938c-156">呼び出しフローと引数は、SDK でサービスとの対話を構造化する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="9938c-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="9938c-157">Azure サービスとの対話の一般的なフローは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="9938c-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="9938c-158">`service.New*Client()` メソッドを使用してクライアントを作成します。`*` は、対話する `service` のリソースの種類です。</span><span class="sxs-lookup"><span data-stu-id="9938c-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="9938c-159">この関数は、常にサブスクリプション ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="9938c-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="9938c-160">クライアントの承認方法を設定して、クライアントがリモート API と対話できるようにします。</span><span class="sxs-lookup"><span data-stu-id="9938c-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="9938c-161">リモート API に対応するクライアントでメソッド呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="9938c-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="9938c-162">通常、サービス クライアント メソッドでは、リソースの名前とメタデータ オブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="9938c-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="9938c-163">ここでは、型変換を実行するために [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 関数が使用されています。</span><span class="sxs-lookup"><span data-stu-id="9938c-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="9938c-164">SDK のメソッドのパラメーターは、ほとんどがポインターを取得するので、型変換を容易にするために便利なメソッドが用意されています。</span><span class="sxs-lookup"><span data-stu-id="9938c-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="9938c-165">便利なコンバーターとその動作の一覧については、[autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) モジュールのドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9938c-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="9938c-166">`groupsClient.CreateOrUpdate` メソッドは、リソース グループを表すデータ型へのポインターを返します。</span><span class="sxs-lookup"><span data-stu-id="9938c-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="9938c-167">この種の直接的な戻り値は、同期する必要がある実行時間の短い操作を示しています。</span><span class="sxs-lookup"><span data-stu-id="9938c-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="9938c-168">次のセクションでは、実行時間の長い操作の例と、その操作と対話する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="9938c-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="9938c-169">デプロイの実行</span><span class="sxs-lookup"><span data-stu-id="9938c-169">Performing the deployment</span></span>

<span data-ttu-id="9938c-170">リソース グループが作成されたら、デプロイを実行します。</span><span class="sxs-lookup"><span data-stu-id="9938c-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="9938c-171">このコードは、ロジックのさまざまな部分を強調するために小さなセクションに分けられています。</span><span class="sxs-lookup"><span data-stu-id="9938c-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="9938c-172">デプロイ ファイルは `readJSON` によって読み込まれますが、その詳細はここでは省略されています。</span><span class="sxs-lookup"><span data-stu-id="9938c-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="9938c-173">この関数は、`*map[string]interface{}` を返します。これは、リソース デプロイの呼び出しに必要なメタデータの作成に使用される型です。</span><span class="sxs-lookup"><span data-stu-id="9938c-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="9938c-174">デプロイ パラメーターで VM のパスワードも手動で設定します。</span><span class="sxs-lookup"><span data-stu-id="9938c-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="9938c-175">このコードは、リソース グループを作成する場合と同じパターンに従っています。</span><span class="sxs-lookup"><span data-stu-id="9938c-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="9938c-176">Azure で認証できることを前提として、新しいクライアントが作成された後、メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="9938c-177">このメソッドは、リソース グループの対応するメソッドと同じ名前 (`CreateOrUpdate`) です。</span><span class="sxs-lookup"><span data-stu-id="9938c-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="9938c-178">このパターンは SDK 全体で見られます。</span><span class="sxs-lookup"><span data-stu-id="9938c-178">This pattern is seen throughout the SDK.</span></span> <span data-ttu-id="9938c-179">同様の処理を実行するメソッドは、通常、同じ名前です。</span><span class="sxs-lookup"><span data-stu-id="9938c-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="9938c-180">最も大きな違いは、`deploymentsClient.CreateOrUpdate` メソッドの戻り値にあります。</span><span class="sxs-lookup"><span data-stu-id="9938c-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="9938c-181">この値は、[Future 設計パターン](https://en.wikipedia.org/wiki/Futures_and_promises)に従う [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) 型です。</span><span class="sxs-lookup"><span data-stu-id="9938c-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="9938c-182">Future は、ポーリング、取り消し、または完了のブロックが可能な、Azure での実行時間の長い操作を表します。</span><span class="sxs-lookup"><span data-stu-id="9938c-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

<span data-ttu-id="9938c-183">この例では、一番良いのは操作が完了するまで待つことです。</span><span class="sxs-lookup"><span data-stu-id="9938c-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="9938c-184">将来のある時点まで待つには、[コンテキスト オブジェクト](https://blog.golang.org/context)と、`Future` オブジェクトを作成したクライアントの両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="9938c-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="9938c-185">ここではエラー ソースとして、メソッドを呼び出そうとしたときにクライアント側で発生したエラーと、サーバーからのエラー応答の 2 つが考えられます。</span><span class="sxs-lookup"><span data-stu-id="9938c-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="9938c-186">後者は、`deploymentFuture.Result` 呼び出しの一部として返されます。</span><span class="sxs-lookup"><span data-stu-id="9938c-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="9938c-187">デプロイ情報を取得したら、データが確実に設定されるように `deploymentsClient.Get` を手動で呼び出すことで、デプロイ情報が空になる可能性のあるバグを回避できます。</span><span class="sxs-lookup"><span data-stu-id="9938c-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="9938c-188">割り当てられた IP アドレスの取得</span><span class="sxs-lookup"><span data-stu-id="9938c-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="9938c-189">新しく作成された VM で 操作を実行するには、割り当てられた IP アドレスが必要です。</span><span class="sxs-lookup"><span data-stu-id="9938c-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="9938c-190">IP アドレスは、ネットワーク インターフェイス コントローラー (NIC) リソースにバインドされた個別の Azure リソースです。</span><span class="sxs-lookup"><span data-stu-id="9938c-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="9938c-191">このメソッドは、パラメーター ファイルに保存されている情報に依存します。</span><span class="sxs-lookup"><span data-stu-id="9938c-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="9938c-192">コードで VM を直接照会して NIC を取得し、NIC を照会して IP リソースを取得してから、IP リソースを直接照会することもできます。</span><span class="sxs-lookup"><span data-stu-id="9938c-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="9938c-193">この場合、依存関係と解決操作の長いチェーンになるため、コストがかかります。</span><span class="sxs-lookup"><span data-stu-id="9938c-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="9938c-194">JSON 情報はローカルであるため、代わりにこの情報を読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="9938c-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="9938c-195">VM ユーザーの値も JSON から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="9938c-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="9938c-196">VM のパスワードは、認証ファイルから既に読み込まれています。</span><span class="sxs-lookup"><span data-stu-id="9938c-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9938c-197">次の手順</span><span class="sxs-lookup"><span data-stu-id="9938c-197">Next steps</span></span>

<span data-ttu-id="9938c-198">このクイック スタートでは、既存のテンプレートを取得し、Go を使用してデプロイしました。</span><span class="sxs-lookup"><span data-stu-id="9938c-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="9938c-199">次に、新しく作成された VM に SSH 経由で接続して、VM が実行されていることを確認しました。</span><span class="sxs-lookup"><span data-stu-id="9938c-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="9938c-200">Go を使用して Azure 環境の仮想マシンを操作する方法について引き続き学習する場合は、[Go 用 Azure コンピューティング サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)または [Go 用 Azure リソース管理サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9938c-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="9938c-201">SDK で使用可能な認証方法と、各認証方法でサポートされる認証の種類の詳細については、[Azure SDK for Go での認証](azure-sdk-go-authorization.md)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9938c-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
