---
title: Azure SDK for Go での認証
description: Azure SDK for Go で使用できる認証方法とそれらの使用方法について説明します。
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.service: active-directory
ms.component: authentication
ms.openlocfilehash: 370f5607b89c0044022f7987d06c3a55c9d6f352
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319885"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a><span data-ttu-id="4fa43-103">Azure SDK for Go における認証方法</span><span class="sxs-lookup"><span data-stu-id="4fa43-103">Authentication methods in the Azure SDK for Go</span></span>

<span data-ttu-id="4fa43-104">Azure SDK for Go では、アプリケーションで使用できるさまざまな認証の種類と方法が提供されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-104">The Azure SDK for Go offers a variety of authentication types and methods that your application can use.</span></span> <span data-ttu-id="4fa43-105">環境変数からの情報の取得から、Web ベースの対話型認証まで、さまざまな認証方法がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4fa43-105">Supported authentication methods range from pulling information from environment variables to interactive web-based authentication.</span></span> <span data-ttu-id="4fa43-106">この記事では、この SDK で使用可能な認証の種類とそれらの使用方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-106">This article introduces you to the available types of authentication in the SDK, and the methods for using them.</span></span> <span data-ttu-id="4fa43-107">また、アプリケーションに適した認証の種類を選択するためのベスト プラクティスについても説明します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-107">You'll also learn best practices for selecting which authentication type is right for your application.</span></span>

## <a name="available-authentication-types-and-methods"></a><span data-ttu-id="4fa43-108">使用可能な認証の種類と方法</span><span class="sxs-lookup"><span data-stu-id="4fa43-108">Available authentication types and methods</span></span>

<span data-ttu-id="4fa43-109">Azure SDK for Go では、異なる資格情報セットを使用する複数の認証の種類が提供されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-109">The Azure SDK for Go offers several different types of authentication, using different credentials sets.</span></span> <span data-ttu-id="4fa43-110">これらの認証の種類は、それぞれ異なる認証方法 (SDK が資格情報を入力として受け取る方法) で使用できます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-110">Each of these authentication types are available through different authentication methods, which are how the SDK takes these credentials as input.</span></span> <span data-ttu-id="4fa43-111">次の表に、使用できる認証の種類と、アプリケーションで使用する際に推奨される状況を示します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-111">The following table describes the available types of authentication and situations in which they're recommended for use by your application.</span></span>

| <span data-ttu-id="4fa43-112">認証の種類</span><span class="sxs-lookup"><span data-stu-id="4fa43-112">Authentication type</span></span> | <span data-ttu-id="4fa43-113">推奨される状況</span><span class="sxs-lookup"><span data-stu-id="4fa43-113">Recommended when...</span></span> |
|---------------------|---------------------|
| <span data-ttu-id="4fa43-114">証明書ベースの認証</span><span class="sxs-lookup"><span data-stu-id="4fa43-114">Certificate-based authentication</span></span> | <span data-ttu-id="4fa43-115">Azure Active Directory (AAD) ユーザーまたはサービス プリンシパル用に構成された X509 証明書がある場合。</span><span class="sxs-lookup"><span data-stu-id="4fa43-115">You have an X509 certificate that was configured for an Azure Active Directory (AAD) user or service principal.</span></span> <span data-ttu-id="4fa43-116">詳細については、「[Azure Active Directory の証明書ベースの認証の概要]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-116">To learn more, see [Get started with certificate-based authentication in Azure Active Directory].</span></span> |
| <span data-ttu-id="4fa43-117">クライアントの資格情報</span><span class="sxs-lookup"><span data-stu-id="4fa43-117">Client credentials</span></span> | <span data-ttu-id="4fa43-118">このアプリケーションまたはアプリケーションが属するクラス用に設定された構成済みのサービス プリンシパルがある場合。</span><span class="sxs-lookup"><span data-stu-id="4fa43-118">You have a configured service principal that is set up for this application or a class of applications it belongs to.</span></span> <span data-ttu-id="4fa43-119">詳細については、[Azure CLI 2.0 でのサービス プリンシパルの作成]に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-119">To learn more, see [Create a service principal with Azure CLI 2.0].</span></span> |
| <span data-ttu-id="4fa43-120">管理対象サービス ID (MSI)</span><span class="sxs-lookup"><span data-stu-id="4fa43-120">Managed Service Identity (MSI)</span></span> | <span data-ttu-id="4fa43-121">管理対象サービス ID (MSI) で構成された Azure リソース上でアプリケーションが実行されている場合。</span><span class="sxs-lookup"><span data-stu-id="4fa43-121">Your application is running on an Azure resource that has been configured with Managed Service Identity (MSI).</span></span> <span data-ttu-id="4fa43-122">詳細については、「[Azure リソースの管理対象サービス ID (MSI)]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-122">To learn more, see [Managed Service Identity (MSI) for Azure resources].</span></span> |
| <span data-ttu-id="4fa43-123">デバイス トークン</span><span class="sxs-lookup"><span data-stu-id="4fa43-123">Device token</span></span> | <span data-ttu-id="4fa43-124">アプリケーションを対話形式で__のみ__使用し、複数の AAD テナントのさまざまなユーザーが使用する可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="4fa43-124">Your application is meant to be used interactively __only__ and will have a variety of users, potentially from multiple AAD tenants.</span></span> <span data-ttu-id="4fa43-125">ユーザーは、Web ブラウザーにアクセスしてログインできます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-125">Users have access to a web browser to log in.</span></span> <span data-ttu-id="4fa43-126">詳細については、「[デバイス トークン認証を使用する](#use-device-token-authentication)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-126">For more information, see [Use device token authentication](#use-device-token-authentication).</span></span>|
| <span data-ttu-id="4fa43-127">ユーザー名/パスワード</span><span class="sxs-lookup"><span data-stu-id="4fa43-127">Username/password</span></span> | <span data-ttu-id="4fa43-128">他のどの認証方法も使用できない対話型アプリケーションがある場合。</span><span class="sxs-lookup"><span data-stu-id="4fa43-128">You have an interactive application that cannot use any other authentication method.</span></span> <span data-ttu-id="4fa43-129">ユーザーは、AAD ログインで多要素認証を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="4fa43-129">Your users do not have multi-factor authentication enabled for their AAD login.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="4fa43-130">クライアントの資格情報以外の認証の種類を使用する場合は、アプリケーションを Azure Active Directory に登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-130">If you use an authentication type other than client credentials, your application must be registered in Azure Active Directory.</span></span> <span data-ttu-id="4fa43-131">方法については、「[Azure Active Directory とアプリケーションの統合](/azure/active-directory/develop/active-directory-integrating-applications)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-131">To learn how, see [Integrating applications with Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).</span></span>

> [!NOTE]
> <span data-ttu-id="4fa43-132">特別な要件がない限り、ユーザー名/パスワード認証は避けてください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-132">Unless you have special requirements, avoid username/password authentication.</span></span> <span data-ttu-id="4fa43-133">ユーザー ベースのログインが適している場合、通常はデバイス トークン認証を代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-133">In situations where user-based login is appropriate, device token authentication can usually be used instead.</span></span>

[Azure Active Directory の証明書ベースの認証の概要]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Get started with certificate-based authentication in Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Azure CLI 2.0 でのサービス プリンシパルの作成]: /cli/azure/create-an-azure-service-principal-azure-cli
[Create a service principal with Azure CLI 2.0]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure リソースの管理対象サービス ID (MSI)]: /azure/active-directory/managed-service-identity/overview
[Managed Service Identity (MSI) for Azure resources]: /azure/active-directory/managed-service-identity/overview

<span data-ttu-id="4fa43-137">これらの認証の種類は、異なる方法で使用できます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-137">These authentication types are available through different methods.</span></span> <span data-ttu-id="4fa43-138">[_環境ベースの認証_](#use-environment-based-authentication)では、プログラムの環境から資格情報を直接読み取ります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-138">[_Environment-based authentication_](#use-environment-based-authentication) reads credentials directly from the program's environment.</span></span> <span data-ttu-id="4fa43-139">[_ファイル ベースの認証_](#use-file-based-authentication)では、サービス プリンシパルの資格情報が含まれたファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-139">[_File-based authentication_](#use-file-based-authentication) loads a file containing service principal credentials.</span></span> <span data-ttu-id="4fa43-140">[_クライアント ベースの認証_](#use-an-authentication-client)では、Go コード内のオブジェクトを使用します。プログラムの実行時に資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-140">[_Client-based authentication_](#use-an-authentication-client) uses an object in Go code and makes you responsible for providing the credentials during program execution.</span></span> <span data-ttu-id="4fa43-141">最後に、[_デバイス トークン認証_](#use-device-token-authentication)では、ユーザーはトークンを使用して Web ブラウザーを介して対話形式でログインする必要があります。環境ベースまたはファイル ベースの認証では使用できません。</span><span class="sxs-lookup"><span data-stu-id="4fa43-141">Finally, [_Device token authentication_](#use-device-token-authentication) requires users to log in interactively through a web browser with a token, and cannot be used with environment- or file-based authentication.</span></span>

<span data-ttu-id="4fa43-142">認証の関数と型はすべて `github.com/Azure/go-autorest/autorest/azure/auth` パッケージで使用できます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-142">All authentication functions and types are available in the `github.com/Azure/go-autorest/autorest/azure/auth` package.</span></span>

> [!NOTE]
> <span data-ttu-id="4fa43-143">特別な要件がない限り、クライアント ベースの認証は避けてください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-143">Unless you have special requirements, avoid client-based authentication.</span></span> <span data-ttu-id="4fa43-144">この認証方法では、バッド プラクティスが助長されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-144">This method of authentication encourages bad practices.</span></span> <span data-ttu-id="4fa43-145">特に、クライアント ベースの認証を使用すると、資格情報をハードコードしてしまいがちになります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-145">In particular, using client-based authentication makes it tempting to hard-code credentials.</span></span> <span data-ttu-id="4fa43-146">また、認証用のカスタム コードを記述すると、認証要件が変わった場合に、SDK の今後のリリースでコードが動作しなくなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-146">Writing custom code for authentication may also break under future SDK releases if authentication requirements change.</span></span>

## <a name="use-environment-based-authentication"></a><span data-ttu-id="4fa43-147">環境ベースの認証を使用する</span><span class="sxs-lookup"><span data-stu-id="4fa43-147">Use environment-based authentication</span></span>

<span data-ttu-id="4fa43-148">コンテナーなどの厳しく管理された環境でアプリケーションを実行する場合は、環境ベースの認証が自然な選択となります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-148">If you're running your application in a tightly controlled environment such as in a container, environment-based authentication is a natural choice.</span></span> <span data-ttu-id="4fa43-149">アプリケーションを実行する前にシェル環境を構成すると、実行時に Go SDK によってこれらの環境変数が読み取られ、Azure で認証が行われます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-149">You configure the shell environment before running your application and the Go SDK reads these environment variables at runtime to authenticate with Azure.</span></span> 

<span data-ttu-id="4fa43-150">環境ベースの認証では、デバイス トークンを除くすべての認証方法がサポートされ、クライアントの資格情報、証明書、ユーザー名/パスワード、管理対象サービス ID (MSI) の順番で評価されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-150">Environment-based authentication has support for all authentication methods except device tokens, evaluated in the following order: Client credentials, certificates, username/password, and Managed Service Identity (MSI).</span></span> <span data-ttu-id="4fa43-151">必要な環境変数が設定されていない場合、または SDK が認証サービスから拒否された場合は、次の認証の種類が試されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-151">If a required environment variable is unset or the SDK gets a refusal from the authentication service, the next authentication type is tried.</span></span> <span data-ttu-id="4fa43-152">SDK が環境から認証できない場合は、エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-152">If the SDK cannot authenticate from the environment, it returns an error.</span></span>

<span data-ttu-id="4fa43-153">次の表に、環境ベースの認証でサポートされている各認証の種類で設定する必要がある環境変数の詳細を示します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-153">The following table details the environment variables that need to be set for each authentication type supported by environment-based authentication.</span></span>

| <span data-ttu-id="4fa43-154">認証の種類</span><span class="sxs-lookup"><span data-stu-id="4fa43-154">Authentication type</span></span> | <span data-ttu-id="4fa43-155">環境変数</span><span class="sxs-lookup"><span data-stu-id="4fa43-155">Environment variable</span></span> | <span data-ttu-id="4fa43-156">[説明]</span><span class="sxs-lookup"><span data-stu-id="4fa43-156">Description</span></span> |
| ------------------- | -------------------- | ----------- |
| <span data-ttu-id="4fa43-157">__クライアントの資格情報__</span><span class="sxs-lookup"><span data-stu-id="4fa43-157">__Client credentials__</span></span> | `AZURE_TENANT_ID` | <span data-ttu-id="4fa43-158">サービス プリンシパルが属する Active Directory テナントの ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-158">The ID for the Active Directory tenant that the service principal belongs to.</span></span> |
| | `AZURE_CLIENT_ID` | <span data-ttu-id="4fa43-159">サービス プリンシパルの名前または ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-159">The name or ID of the service principal.</span></span> |
| | `AZURE_CLIENT_SECRET` | <span data-ttu-id="4fa43-160">サービス プリンシパルに関連付けられているシークレット。</span><span class="sxs-lookup"><span data-stu-id="4fa43-160">The secret associated with the service principal.</span></span> |
| <span data-ttu-id="4fa43-161">__証明書__</span><span class="sxs-lookup"><span data-stu-id="4fa43-161">__Certificate__</span></span> | `AZURE_TENANT_ID` | <span data-ttu-id="4fa43-162">証明書が登録されている Active Directory テナントの ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-162">The ID for the Active Directory tenant that the certificate is registered with.</span></span> |
| | `AZURE_CLIENT_ID` | <span data-ttu-id="4fa43-163">証明書に関連付けられているアプリケーション クライアント ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-163">The application client ID associated with the certificate.</span></span> |
| | `AZURE_CERTIFICATE_PATH` | <span data-ttu-id="4fa43-164">クライアント証明書ファイルのパス。</span><span class="sxs-lookup"><span data-stu-id="4fa43-164">The path to the client certificate file.</span></span> |
| | `AZURE_CERTIFICATE_PASSWORD` | <span data-ttu-id="4fa43-165">クライアント証明書のパスワード。</span><span class="sxs-lookup"><span data-stu-id="4fa43-165">The password for the client certificate.</span></span> |
| <span data-ttu-id="4fa43-166">__ユーザー名/パスワード__</span><span class="sxs-lookup"><span data-stu-id="4fa43-166">__Username/Password__</span></span> | `AZURE_TENANT_ID` | <span data-ttu-id="4fa43-167">ユーザーが属する Active Directory テナントの ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-167">The ID for the Active Directory tenant that the user belongs to.</span></span> |
| | `AZURE_CLIENT_ID` | <span data-ttu-id="4fa43-168">アプリケーション クライアント ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-168">The application client ID.</span></span> |
| | `AZURE_USERNAME` | <span data-ttu-id="4fa43-169">ログインに使用するユーザー名。</span><span class="sxs-lookup"><span data-stu-id="4fa43-169">The username to log in with.</span></span> |
| | `AZURE_PASSWORD` | <span data-ttu-id="4fa43-170">ログインに使用するパスワード。</span><span class="sxs-lookup"><span data-stu-id="4fa43-170">The password to log in with.</span></span> |
| <span data-ttu-id="4fa43-171">__MSI__</span><span class="sxs-lookup"><span data-stu-id="4fa43-171">__MSI__</span></span> | | <span data-ttu-id="4fa43-172">MSI では、資格情報を設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4fa43-172">MSI does not require any credentials to be set.</span></span> <span data-ttu-id="4fa43-173">アプリケーションは、MSI を使用するように構成された Azure リソース上で実行されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-173">The application must be running on an Azure resource configured to use MSI.</span></span> <span data-ttu-id="4fa43-174">詳細については、「[Azure リソースの管理対象サービス ID (MSI)]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-174">For details, see [Managed Service Identity (MSI) for Azure resources].</span></span> |

<span data-ttu-id="4fa43-175">既定の Azure パブリック クラウド以外のクラウドまたは管理エンドポイントに接続する必要がある場合は、次の環境変数を設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-175">If you need to connect to a cloud or management endpoint other than the default Azure public cloud, you can also set the following environment variables.</span></span> <span data-ttu-id="4fa43-176">これらの環境変数は、Azure Stack、異なる地理的リージョンのクラウド、または Azure クラシック デプロイ モデルを使用する場合に設定するのが最も一般的です。</span><span class="sxs-lookup"><span data-stu-id="4fa43-176">The most common reasons to set them are if you use Azure Stack, a cloud in a different geographic region, or the Azure Classic deployment model.</span></span>

| <span data-ttu-id="4fa43-177">環境変数</span><span class="sxs-lookup"><span data-stu-id="4fa43-177">Environment variable</span></span> | <span data-ttu-id="4fa43-178">[説明]</span><span class="sxs-lookup"><span data-stu-id="4fa43-178">Description</span></span>  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | <span data-ttu-id="4fa43-179">接続先のクラウド環境の名前。</span><span class="sxs-lookup"><span data-stu-id="4fa43-179">The name of the cloud environment to connect to.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="4fa43-180">接続時に使用する Active Directory リソース ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-180">The Active Directory resource ID to use when connecting.</span></span> <span data-ttu-id="4fa43-181">これは、管理エンドポイントを参照する URI である必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-181">This should be a URI pointing to your management endpoint.</span></span> |

<span data-ttu-id="4fa43-182">環境ベースの認証を使用する場合は、[NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) 関数を呼び出して authorizer オブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-182">When using environment-based authentication, call the [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) function to get your authorizer object.</span></span> <span data-ttu-id="4fa43-183">その後、このオブジェクトがクライアントの `Authorizer` プロパティで設定され、Azure へのクライアントのアクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-183">This object is then set on the `Authorizer` property of clients to allow them access to Azure.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a><span data-ttu-id="4fa43-184">Azure Stack での認証</span><span class="sxs-lookup"><span data-stu-id="4fa43-184">Authentication on Azure Stack</span></span>

<span data-ttu-id="4fa43-185">Azure Stack で認証するには、次の変数を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-185">To authenticate on Azure Stack, you need to set the following variables:</span></span>

| <span data-ttu-id="4fa43-186">環境変数</span><span class="sxs-lookup"><span data-stu-id="4fa43-186">Environment variable</span></span> | <span data-ttu-id="4fa43-187">[説明]</span><span class="sxs-lookup"><span data-stu-id="4fa43-187">Description</span></span>  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | <span data-ttu-id="4fa43-188">Active Directory エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="4fa43-188">The Active Directory endpoint.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="4fa43-189">Active Directory リソース ID。</span><span class="sxs-lookup"><span data-stu-id="4fa43-189">The Active Directory resource ID.</span></span> |

<span data-ttu-id="4fa43-190">これらの変数は、Azure Stack のメタデータ情報から取得できます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-190">These variables can be retrieved from Azure Stack metadata information.</span></span> <span data-ttu-id="4fa43-191">メタデータを取得するには、Azure Stack 環境で Web ブラウザーを開き、URL として `(ResourceManagerURL)/metadata/endpoints?api-version=1.0` を使用します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-191">To retrieve the metadata, open a web browser in your Azure Stack environment and use the url: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`</span></span>

<span data-ttu-id="4fa43-192">`ResourceManagerURL` は、Azure Stack デプロイのリージョン名、マシン名、外部完全修飾ドメイン名 (FQDN) によって異なります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-192">The `ResourceManagerURL` varies based on the region name, machine name and external fully qualified domain name (FQDN) of your Azure Stack deployment:</span></span>

| <span data-ttu-id="4fa43-193">環境</span><span class="sxs-lookup"><span data-stu-id="4fa43-193">Environment</span></span> | <span data-ttu-id="4fa43-194">ResourceManagerURL</span><span class="sxs-lookup"><span data-stu-id="4fa43-194">ResourceManagerURL</span></span> |
|----------------------|--------------|
| <span data-ttu-id="4fa43-195">開発キット</span><span class="sxs-lookup"><span data-stu-id="4fa43-195">Development Kit</span></span> | `https://management.local.azurestack.external/` |
| <span data-ttu-id="4fa43-196">統合システム</span><span class="sxs-lookup"><span data-stu-id="4fa43-196">Integrated Systems</span></span> | `https://management.(region).ext-(machine-name).(FQDN)` |

<span data-ttu-id="4fa43-197">Azure Stack での Azure SDK for Go の使用方法の詳細については、「[Azure Stack での GO による API バージョンのプロファイルの使用](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-version-profiles-go)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-197">For more details on how to use Azure SDK for Go on Azure Stack see [Use API version profiles with Go in Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-version-profiles-go)</span></span>


## <a name="use-file-based-authentication"></a><span data-ttu-id="4fa43-198">ファイル ベースの認証を使用する</span><span class="sxs-lookup"><span data-stu-id="4fa43-198">Use file-based authentication</span></span>

<span data-ttu-id="4fa43-199">ファイル ベースの認証は、[Azure CLI 2.0](/cli/azure) で生成されたローカル ファイル形式で保存されているクライアントの資格情報でのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-199">File-based authentication only works with client credentials when they are stored in a local file format generated by [the Azure CLI 2.0](/cli/azure).</span></span> <span data-ttu-id="4fa43-200">`--sdk-auth` パラメーターを指定して新しいサービス プリンシパルを作成すると、このファイルを簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-200">You can easily create this file when creating a new service principal with the `--sdk-auth` parameter.</span></span> <span data-ttu-id="4fa43-201">ファイル ベースの認証を使用する場合は、サービス プリンシパルの作成時に、この引数が指定されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-201">If you plan on using file-based authentication, make sure that this argument is provided when creating a service principal.</span></span> <span data-ttu-id="4fa43-202">CLI では出力が `stdout` に生成されるので、出力をファイルにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="4fa43-202">Since the CLI prints output to `stdout`, redirect output to a file.</span></span>

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

<span data-ttu-id="4fa43-203">`AZURE_AUTH_LOCATION` 環境変数を、承認ファイルがある場所に設定します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-203">Set the `AZURE_AUTH_LOCATION` environment variable to where the authorization file is located.</span></span> <span data-ttu-id="4fa43-204">この環境変数をアプリケーションが読み取り、環境変数内の資格情報が解析されます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-204">This environment variable is read by the application, and the credentials within it are parsed.</span></span> <span data-ttu-id="4fa43-205">承認ファイルを実行時に選択する必要がある場合は、[os.Setenv](https://golang.org/pkg/os/#Setenv) 関数を使用してプログラムの環境を操作します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-205">If you need to select the authorization file at runtime, manipulate the program's environment using the [os.Setenv](https://golang.org/pkg/os/#Setenv) function.</span></span>

<span data-ttu-id="4fa43-206">認証情報を読み込むには、[NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-206">To load the authentication information, call the [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) function.</span></span> <span data-ttu-id="4fa43-207">環境ベースの承認とは異なり、ファイル ベースの承認にはリソース エンドポイントが必要です。</span><span class="sxs-lookup"><span data-stu-id="4fa43-207">Unlike environment-based authorization, file-based authorization requires a resource endpoint.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

<span data-ttu-id="4fa43-208">サービス プリンシパルの使用とアクセス許可の管理の詳細については、[Azure CLI 2.0 でのサービス プリンシパルの作成]に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-208">For more on using service principals and managing their access permissions, see [Create a service principal with Azure CLI 2.0].</span></span>

## <a name="use-device-token-authentication"></a><span data-ttu-id="4fa43-209">デバイス トークン認証を使用する</span><span class="sxs-lookup"><span data-stu-id="4fa43-209">Use device token authentication</span></span>

<span data-ttu-id="4fa43-210">ユーザーが対話形式でログインできるようにする場合、この機能を提供する最良の方法は、デバイス トークン認証を使用することです。</span><span class="sxs-lookup"><span data-stu-id="4fa43-210">If you want users to log in interactively, the best way to offer that capability is through device token authentication.</span></span> <span data-ttu-id="4fa43-211">この認証フローでは、Microsoft ログイン サイトに貼り付けるトークンをユーザーに渡し、ユーザーは Azure Active Directory (AAD) アカウントでログインします。</span><span class="sxs-lookup"><span data-stu-id="4fa43-211">This authentication flow passes the user a token to paste into a Microsoft login site, where they then log in with an Azure Active Directory (AAD) account.</span></span> <span data-ttu-id="4fa43-212">標準のユーザー名/パスワード認証とは異なり、この認証方法では多要素認証が有効になっているアカウントがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-212">This authentication method supports accounts that have multi-factor authentication enabled, unlike standard username/password authentication.</span></span>

<span data-ttu-id="4fa43-213">デバイス トークン認証を使用するには、[NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) 関数で [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) Authorizer を作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-213">To use device token authentication, create a [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) authorizer with the [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) function.</span></span> <span data-ttu-id="4fa43-214">作成されたオブジェクトで [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) を呼び出して認証プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-214">Call [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) on the resulting object to start the authentication process.</span></span> <span data-ttu-id="4fa43-215">デバイス認証フローでは、認証フロー全体が完了するまでプログラムの実行がブロックされます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-215">Device flow authentication blocks program execution until the whole authentication flow is complete.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a><span data-ttu-id="4fa43-216">認証クライアントを使用する</span><span class="sxs-lookup"><span data-stu-id="4fa43-216">Use an authentication client</span></span>

<span data-ttu-id="4fa43-217">特定の種類の認証が必要であり、ユーザーの認証情報を読み込む処理をプログラムで実行する場合は、[auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) インターフェイスに準拠したクライアントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-217">If you require a specific type of authentication and are willing to have your program do the work to load authentication information from the user, you can use any client that conforms to the [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) interface.</span></span> <span data-ttu-id="4fa43-218">対話型プログラムが必要な場合、特別な構成ファイルを使用する場合、または別の認証方法を使用できない要件がある場合は、このインターフェイスを実装する型を使用します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-218">Use a type that implements this interface when you want an interactive program, use specialized configuration files, or have a requirement that prevents you from using another authentication method.</span></span>

> [!WARNING]
> <span data-ttu-id="4fa43-219">Azure の資格情報をアプリケーションにハードコードしないでください。</span><span class="sxs-lookup"><span data-stu-id="4fa43-219">Never hard-code Azure credentials into an application.</span></span> <span data-ttu-id="4fa43-220">シークレットをアプリケーションのバイナリに配置すると、アプリケーションが実行されているかどうかに関係なく、攻撃者がシークレットを簡単に抽出できるようになります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-220">Putting secrets into an application binary makes it easier for an attacker to extract them, whether the application is running or not.</span></span> <span data-ttu-id="4fa43-221">この場合、資格情報が承認されているすべての Azure リソースが危険にさらされます。</span><span class="sxs-lookup"><span data-stu-id="4fa43-221">This puts all Azure resources the credentials are authorized for at risk!</span></span>

<span data-ttu-id="4fa43-222">次の表に、`AuthorizerConfig` インターフェイスに準拠した SDK の型を示します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-222">The following table lists the types in the SDK that conform to the `AuthorizerConfig` interface.</span></span>

| <span data-ttu-id="4fa43-223">認証の種類</span><span class="sxs-lookup"><span data-stu-id="4fa43-223">Authentication type</span></span> | <span data-ttu-id="4fa43-224">Authorizer の型</span><span class="sxs-lookup"><span data-stu-id="4fa43-224">Authorizer type</span></span> |
|---------------------|-----------------------|
| <span data-ttu-id="4fa43-225">証明書ベースの認証</span><span class="sxs-lookup"><span data-stu-id="4fa43-225">Certificate-based authentication</span></span> | <span data-ttu-id="4fa43-226">[ClientCertificateConfig]</span><span class="sxs-lookup"><span data-stu-id="4fa43-226">[ClientCertificateConfig]</span></span> |
| <span data-ttu-id="4fa43-227">クライアントの資格情報</span><span class="sxs-lookup"><span data-stu-id="4fa43-227">Client credentials</span></span> | <span data-ttu-id="4fa43-228">[ClientCredentialsConfig]</span><span class="sxs-lookup"><span data-stu-id="4fa43-228">[ClientCredentialsConfig]</span></span> |
| <span data-ttu-id="4fa43-229">管理対象サービス ID (MSI)</span><span class="sxs-lookup"><span data-stu-id="4fa43-229">Managed Service Identity (MSI)</span></span> | <span data-ttu-id="4fa43-230">[MSIConfig]</span><span class="sxs-lookup"><span data-stu-id="4fa43-230">[MSIConfig]</span></span> |
| <span data-ttu-id="4fa43-231">ユーザー名/パスワード</span><span class="sxs-lookup"><span data-stu-id="4fa43-231">Username/password</span></span> | <span data-ttu-id="4fa43-232">[UsernamePasswordConfig]</span><span class="sxs-lookup"><span data-stu-id="4fa43-232">[UsernamePasswordConfig]</span></span> |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

<span data-ttu-id="4fa43-237">関連付けられた `New` 関数で認証子を作成し、作成されたオブジェクトで `Authorize` を呼び出して認証を実行します。</span><span class="sxs-lookup"><span data-stu-id="4fa43-237">Create an authenticator with its associated `New` function, and then call `Authorize` on the resulting object to perform authentication.</span></span> <span data-ttu-id="4fa43-238">たとえば、証明書ベースの認証を使用する場合は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4fa43-238">For example, to use certificate-based authentication:</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
