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
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="bbd5f-103">クイック スタート: Azure SDK for Go を使用してテンプレートから Azure 仮想マシンをデプロイする</span><span class="sxs-lookup"><span data-stu-id="bbd5f-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="bbd5f-104">このクイック スタートでは、Azure SDK for Go を使用して Azure Resource Manager テンプレートからリソースをデプロイする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="bbd5f-105">テンプレートは、[Azure リソース グループ](/azure/azure-resource-manager/resource-group-overview)内のすべてのリソースのスナップショットです。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="bbd5f-106">この過程で、SDK の機能と規則について理解を深めます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="bbd5f-107">このクイック スタートの終わりには、実行中の VM にユーザー名とパスワードを使用してログインした状態になります。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="bbd5f-108">Resource Manager テンプレートを使用せずに Go 内で VM を作成する方法を確認する場合は、この SDK を使用して VM のすべてのリソースを作成して構成する方法を説明した[命令型のサンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go)があります。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="bbd5f-109">このサンプルのテンプレートを使用すると、Azure サービスのアーキテクチャに関する数多くの詳細に深入りすることなく、SDK の規則に集中できます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="bbd5f-110">Azure CLI のローカル インストールを使用する場合、このクイック スタートには CLI バージョン __2.0.28__ 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="bbd5f-111">`az --version` を実行して、CLI インストールがこの要件を満たしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="bbd5f-112">インストールまたはアップグレードが必要な場合は、[Azure CLI のインストール](/cli/azure/install-azure-cli)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="bbd5f-113">Azure SDK for Go のインストール</span><span class="sxs-lookup"><span data-stu-id="bbd5f-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="bbd5f-114">サービス プリンシパルの作成</span><span class="sxs-lookup"><span data-stu-id="bbd5f-114">Create a service principal</span></span>

<span data-ttu-id="bbd5f-115">アプリケーションで非対話形式で Azure にサインインするには、サービス プリンシパルが必要です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="bbd5f-116">サービス プリンシパルはロールベースのアクセス制御 (RBAC) の一部であり、これによって一意のユーザー ID が作成されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="bbd5f-117">CLI で新しいサービス プリンシパルを作成するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="bbd5f-118">`AZURE_AUTH_LOCATION` 環境変数を、このファイルの完全なパスに設定します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="bbd5f-119">これにより、SDK によってこのファイルが検索され、ファイルから資格情報が直接読み取られます。サービス プリンシパルの情報を変更したり、記録したりする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="bbd5f-120">コードの入手</span><span class="sxs-lookup"><span data-stu-id="bbd5f-120">Get the code</span></span>

<span data-ttu-id="bbd5f-121">`go get` を使用して、クイック スタート コードとすべての依存関係を入手します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="bbd5f-122">`AZURE_AUTH_LOCATION` 変数が適切に設定されていれば、ソース コードを変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="bbd5f-123">プログラムが実行されると、必要なすべての認証情報がそこから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="bbd5f-124">コードの実行</span><span class="sxs-lookup"><span data-stu-id="bbd5f-124">Running the code</span></span>

<span data-ttu-id="bbd5f-125">`go run` コマンドを使用してクイック スタートを実行します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="bbd5f-126">デプロイが成功すると、新しく作成された仮想マシンにログインするためのユーザー名、IP アドレス、パスワードを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="bbd5f-127">このマシンに SSH 接続し、マシンが起動して動作しているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="bbd5f-128">クリーンアップしています</span><span class="sxs-lookup"><span data-stu-id="bbd5f-128">Cleaning up</span></span>

<span data-ttu-id="bbd5f-129">このクイック スタートで作成したリソースをクリーンアップするには、CLI を使用してリソース グループを削除します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="bbd5f-130">作成されたサービス プリンシパルも削除します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="bbd5f-131">`quickstart.auth` ファイルに `clientId` の JSON キーがあります。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="bbd5f-132">この値を `CLIENT_ID_VALUE` 環境変数にコピーし、次の Azure CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="bbd5f-133">ここに `quickstart.auth` の `CLIENT_ID_VALUE` の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="bbd5f-134">このアプリケーションのサービス プリンシパルの削除に失敗すると、アプリケーションは Azure Active Directory テナントでアクティブなままになります。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="bbd5f-135">このサービス プリンシパルの名前とパスワードの両方が UUID として生成されますが、使用されないサービス プリンシパルと Azure Active Directory アプリケーションをすべて削除することで、適切なセキュリティ プラクティスに必ず従ってください。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="bbd5f-136">コードの詳細</span><span class="sxs-lookup"><span data-stu-id="bbd5f-136">Code in depth</span></span>

<span data-ttu-id="bbd5f-137">クイック スタート コードの内容は、変数のブロックといくつかの小さな関数に分かれています。ここでは、それぞれについて説明します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="bbd5f-138">変数、定数、型</span><span class="sxs-lookup"><span data-stu-id="bbd5f-138">Variables, constants, and types</span></span>

<span data-ttu-id="bbd5f-139">クイック スタートは自己完結型であるため、グローバル定数とグローバル変数が使用されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="bbd5f-140">作成するリソースの名前を指定する値が宣言されています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="bbd5f-141">ここでは場所も指定されています。この場所を変更して、他のデータセンターでのデプロイの動作を確認できます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="bbd5f-142">すべてのデータセンターで、必要なリソースがすべて提供されるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="bbd5f-143">`clientInfo` 型には、SDK でクライアントをセットアップし、VM のパスワードを設定するために、認証ファイルから読み込まれた情報が保持されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="bbd5f-144">定数 `templateFile` と `parametersFile` は、デプロイに必要なファイルを参照しています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="bbd5f-145">`authorizer` は、認証のために Go SDK によって構成されます。`ctx` 変数は、ネットワーク操作の [Go コンテキスト](https://blog.golang.org/context)です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="bbd5f-146">認証と初期化</span><span class="sxs-lookup"><span data-stu-id="bbd5f-146">Authentication and initialization</span></span>

<span data-ttu-id="bbd5f-147">`init` 関数では認証が設定されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="bbd5f-148">認証はこのクイック スタートのあらゆるものの前提条件となるため、初期化に含めるのが合理的です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="bbd5f-149">また、クライアントと VM を構成するために必要な情報が認証ファイルから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="bbd5f-150">まず、[auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) が呼び出され、`AZURE_AUTH_LOCATION` にあるファイルから認証情報が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="bbd5f-151">次に、`readJSON` 関数によってこのファイルが手動で読み込まれ (ここでは省略)、プログラムの残りの部分を実行するために必要な 2 つの値 (クライアントのサブスクリプション ID と、VM のパスワードにも使用されるサービス プリンシパルのシークレット) が取得されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="bbd5f-152">このクイック スタートをシンプルに保つために、サービス プリンシパルのパスワードが再利用されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="bbd5f-153">運用環境では、Azure リソースにアクセスするためのパスワードを__再利用しない__ように注意してください。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="bbd5f-154">main() の操作のフロー</span><span class="sxs-lookup"><span data-stu-id="bbd5f-154">Flow of operations in main()</span></span>

<span data-ttu-id="bbd5f-155">`main` は、操作のフローを示し、エラー チェックを実行するだけの単純な関数です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="bbd5f-156">コードで実行される手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="bbd5f-157">デプロイ先のリソース グループを作成する (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="bbd5f-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="bbd5f-158">このグループ内にデプロイを作成する (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="bbd5f-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="bbd5f-159">デプロイした VM のログイン情報を取得して表示する (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="bbd5f-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="bbd5f-160">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="bbd5f-160">Create the resource group</span></span>

<span data-ttu-id="bbd5f-161">`createGroup` 関数はリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="bbd5f-162">呼び出しフローと引数は、SDK でサービスとの対話を構造化する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="bbd5f-163">Azure サービスとの対話の一般的なフローは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="bbd5f-164">`service.New*Client()` メソッドを使用してクライアントを作成します。`*` は、対話する `service` のリソースの種類です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="bbd5f-165">この関数は、常にサブスクリプション ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="bbd5f-166">クライアントの承認方法を設定して、クライアントがリモート API と対話できるようにします。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="bbd5f-167">リモート API に対応するクライアントでメソッド呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="bbd5f-168">通常、サービス クライアント メソッドでは、リソースの名前とメタデータ オブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="bbd5f-169">ここでは、型変換を実行するために [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 関数が使用されています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="bbd5f-170">SDK のメソッドのパラメーターは、ほとんどがポインターを取得するので、型変換を容易にするために便利なメソッドが用意されています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="bbd5f-171">便利なコンバーターとその動作の一覧については、[autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) モジュールのドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="bbd5f-172">`groupsClient.CreateOrUpdate` メソッドは、リソース グループを表すデータ型へのポインターを返します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="bbd5f-173">この種の直接的な戻り値は、同期する必要がある実行時間の短い操作を示しています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="bbd5f-174">次のセクションでは、実行時間の長い操作の例と、その操作と対話する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="bbd5f-175">デプロイの実行</span><span class="sxs-lookup"><span data-stu-id="bbd5f-175">Perform the deployment</span></span>

<span data-ttu-id="bbd5f-176">リソース グループが作成されたら、デプロイを実行します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="bbd5f-177">このコードは、ロジックのさまざまな部分を強調するために小さなセクションに分けられています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="bbd5f-178">デプロイ ファイルは `readJSON` によって読み込まれますが、その詳細はここでは省略されています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="bbd5f-179">この関数は、`*map[string]interface{}` を返します。これは、リソース デプロイの呼び出しに必要なメタデータの作成に使用される型です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="bbd5f-180">デプロイ パラメーターで VM のパスワードも手動で設定します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-180">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="bbd5f-181">このコードは、リソース グループを作成する場合と同じパターンに従っています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="bbd5f-182">Azure で認証できることを前提として、新しいクライアントが作成された後、メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="bbd5f-183">このメソッドは、リソース グループの対応するメソッドと同じ名前 (`CreateOrUpdate`) です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="bbd5f-184">このパターンは SDK 全体で見られます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="bbd5f-185">同様の処理を実行するメソッドは、通常、同じ名前です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="bbd5f-186">最も大きな違いは、`deploymentsClient.CreateOrUpdate` メソッドの戻り値にあります。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="bbd5f-187">この値は、[Future 設計パターン](https://en.wikipedia.org/wiki/Futures_and_promises)に従う [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) 型です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="bbd5f-188">Future は、ポーリング、取り消し、または完了のブロックが可能な、Azure での実行時間の長い操作を表します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="bbd5f-189">この例では、一番良いのは操作が完了するまで待つことです。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="bbd5f-190">将来のある時点まで待つには、[コンテキスト オブジェクト](https://blog.golang.org/context)と、`Future` オブジェクトを作成したクライアントの両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="bbd5f-191">ここではエラー ソースとして、メソッドを呼び出そうとしたときにクライアント側で発生したエラーと、サーバーからのエラー応答の 2 つが考えられます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="bbd5f-192">後者は、`deploymentFuture.Result` 呼び出しの一部として返されます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="bbd5f-193">割り当てられた IP アドレスの取得</span><span class="sxs-lookup"><span data-stu-id="bbd5f-193">Get the assigned IP address</span></span>

<span data-ttu-id="bbd5f-194">新しく作成された VM で 操作を実行するには、割り当てられた IP アドレスが必要です。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="bbd5f-195">IP アドレスは、ネットワーク インターフェイス コントローラー (NIC) リソースにバインドされた個別の Azure リソースです。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="bbd5f-196">このメソッドは、パラメーター ファイルに保存されている情報に依存します。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="bbd5f-197">コードで VM を直接照会して NIC を取得し、NIC を照会して IP リソースを取得してから、IP リソースを直接照会することもできます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="bbd5f-198">この場合、依存関係と解決操作の長いチェーンになるため、コストがかかります。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="bbd5f-199">JSON 情報はローカルであるため、代わりにこの情報を読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="bbd5f-200">VM ユーザーの値も JSON から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="bbd5f-201">VM のパスワードは、認証ファイルから既に読み込まれています。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbd5f-202">次の手順</span><span class="sxs-lookup"><span data-stu-id="bbd5f-202">Next steps</span></span>

<span data-ttu-id="bbd5f-203">このクイック スタートでは、既存のテンプレートを取得し、Go を使用してデプロイしました。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="bbd5f-204">次に、新しく作成された VM に SSH 経由で接続しました。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="bbd5f-205">Go を使用して Azure 環境の仮想マシンを操作する方法について引き続き学習する場合は、[Go 用 Azure コンピューティング サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)または [Go 用 Azure リソース管理サンプル](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="bbd5f-206">SDK で使用可能な認証方法と、各認証方法でサポートされる認証の種類の詳細については、[Azure SDK for Go での認証](azure-sdk-go-authorization.md)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bbd5f-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
