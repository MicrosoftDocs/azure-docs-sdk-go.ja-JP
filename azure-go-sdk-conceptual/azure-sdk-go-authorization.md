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
ms.component: authentication
ms.openlocfilehash: c2c3dccfa8da5cfe57fee0b90139002068982560
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2018
ms.locfileid: "49481984"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a><span data-ttu-id="28e5f-103">Azure SDK for Go における認証方法</span><span class="sxs-lookup"><span data-stu-id="28e5f-103">Authentication methods in the Azure SDK for Go</span></span>

<span data-ttu-id="28e5f-104">Azure SDK for Go には、Azure での認証方法が複数用意されています。</span><span class="sxs-lookup"><span data-stu-id="28e5f-104">The Azure SDK for Go offers multiple ways to authenticate with Azure.</span></span> <span data-ttu-id="28e5f-105">これらの認証の "_種類_" は、さまざまな認証 "_方法_" で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-105">These authentication _types_ are invoked through different authentication _methods_.</span></span> <span data-ttu-id="28e5f-106">この記事では、使用可能な種類、方法、およびアプリケーションに最適なものを選択する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-106">This article covers the available types, methods, and how to choose which are best for your application.</span></span>

## <a name="available-authentication-types-and-methods"></a><span data-ttu-id="28e5f-107">使用可能な認証の種類と方法</span><span class="sxs-lookup"><span data-stu-id="28e5f-107">Available authentication types and methods</span></span>

<span data-ttu-id="28e5f-108">Azure SDK for Go では、異なる資格情報セットを使用する複数の認証の種類が提供されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-108">The Azure SDK for Go offers several different types of authentication, using different credentials sets.</span></span> <span data-ttu-id="28e5f-109">認証の種類は、それぞれ異なる認証方法 (SDK が資格情報を入力として受け取る方法) で使用できます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-109">Each authentication type is available through different authentication methods, which are how the SDK takes these credentials as input.</span></span> <span data-ttu-id="28e5f-110">次の表に、使用できる認証の種類と、アプリケーションで使用する際に推奨される状況を示します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-110">The following table describes the available types of authentication and situations in which they're recommended for use by your application.</span></span>

| <span data-ttu-id="28e5f-111">認証の種類</span><span class="sxs-lookup"><span data-stu-id="28e5f-111">Authentication type</span></span> | <span data-ttu-id="28e5f-112">推奨される状況</span><span class="sxs-lookup"><span data-stu-id="28e5f-112">Recommended when...</span></span> |
|---------------------|---------------------|
| <span data-ttu-id="28e5f-113">証明書ベースの認証</span><span class="sxs-lookup"><span data-stu-id="28e5f-113">Certificate-based authentication</span></span> | <span data-ttu-id="28e5f-114">Azure Active Directory (AAD) ユーザーまたはサービス プリンシパル用に構成された X509 証明書がある場合。</span><span class="sxs-lookup"><span data-stu-id="28e5f-114">You have an X509 certificate that was configured for an Azure Active Directory (AAD) user or service principal.</span></span> <span data-ttu-id="28e5f-115">詳細については、「[Azure Active Directory の証明書ベースの認証の概要]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-115">To learn more, see [Get started with certificate-based authentication in Azure Active Directory].</span></span> |
| <span data-ttu-id="28e5f-116">クライアントの資格情報</span><span class="sxs-lookup"><span data-stu-id="28e5f-116">Client credentials</span></span> | <span data-ttu-id="28e5f-117">このアプリケーションまたはアプリケーションが属するクラス用に設定された構成済みのサービス プリンシパルがある場合。</span><span class="sxs-lookup"><span data-stu-id="28e5f-117">You have a configured service principal that is set up for this application or a class of applications it belongs to.</span></span> <span data-ttu-id="28e5f-118">詳細については、[Azure CLI でのサービス プリンシパルの作成]に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-118">To learn more, see [Create a service principal with Azure CLI].</span></span> |
| <span data-ttu-id="28e5f-119">Azure リソースのマネージド ID</span><span class="sxs-lookup"><span data-stu-id="28e5f-119">Managed identities for Azure resources</span></span> | <span data-ttu-id="28e5f-120">マネージド ID で構成された Azure リソース上でアプリケーションが実行されている場合。</span><span class="sxs-lookup"><span data-stu-id="28e5f-120">Your application is running on an Azure resource that has been configured with a managed identity.</span></span> <span data-ttu-id="28e5f-121">詳細については、[Azure リソースのマネージド ID]に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-121">To learn more, see [Managed identities for Azure resources].</span></span> |
| <span data-ttu-id="28e5f-122">デバイス トークン</span><span class="sxs-lookup"><span data-stu-id="28e5f-122">Device token</span></span> | <span data-ttu-id="28e5f-123">お使いのアプリケーションでは、対話形式 "__のみ__" の使用が想定されている場合。</span><span class="sxs-lookup"><span data-stu-id="28e5f-123">Your application is meant to be used interactively __only__.</span></span> <span data-ttu-id="28e5f-124">ユーザーが多要素認証を有効にしていることがあります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-124">Users may have multi-factor authentication enabled.</span></span> <span data-ttu-id="28e5f-125">ユーザーは、Web ブラウザーにアクセスしてサインインできます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-125">Users have access to a web browser to sign in.</span></span> <span data-ttu-id="28e5f-126">詳細については、「[デバイス トークン認証を使用する](#use-device-token-authentication)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-126">For more information, see [Use device token authentication](#use-device-token-authentication).</span></span>|
| <span data-ttu-id="28e5f-127">ユーザー名/パスワード</span><span class="sxs-lookup"><span data-stu-id="28e5f-127">Username/password</span></span> | <span data-ttu-id="28e5f-128">他のどの認証方法も使用できない対話型アプリケーションがある場合。</span><span class="sxs-lookup"><span data-stu-id="28e5f-128">You have an interactive application that can't use any other authentication method.</span></span> <span data-ttu-id="28e5f-129">ユーザーは、AAD サインインで多要素認証を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="28e5f-129">Your users don't have multi-factor authentication enabled for their AAD sign-in.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="28e5f-130">クライアントの資格情報以外の認証の種類を使用する場合は、アプリケーションを Azure Active Directory に登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-130">If you use an authentication type other than client credentials, your application must be registered in Azure Active Directory.</span></span> <span data-ttu-id="28e5f-131">方法については、「[Azure Active Directory とアプリケーションの統合](/azure/active-directory/develop/active-directory-integrating-applications)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-131">To learn how, see [Integrating applications with Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).</span></span>
>
> [!NOTE]
> <span data-ttu-id="28e5f-132">特別な要件がない限り、ユーザー名/パスワード認証は避けてください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-132">Unless you have special requirements, avoid username/password authentication.</span></span> <span data-ttu-id="28e5f-133">ユーザー ベースのサインインが適している場合、通常はデバイス トークン認証を代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-133">In situations where user-based sign in is appropriate, device token authentication can usually be used instead.</span></span>

[Azure Active Directory の証明書ベースの認証の概要]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Get started with certificate-based authentication in Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Azure CLI でのサービス プリンシパルの作成]: /cli/azure/create-an-azure-service-principal-azure-cli
[Create a service principal with Azure CLI]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure リソースのマネージド ID]: /azure/active-directory/managed-identities-azure-resources/overview
[Managed identities for Azure resources]: /azure/active-directory/managed-identities-azure-resources/overview

<span data-ttu-id="28e5f-137">これらの認証の種類は、異なる方法で使用できます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-137">These authentication types are available through different methods.</span></span>

* <span data-ttu-id="28e5f-138">[_環境ベースの認証_](#use-environment-based-authentication)では、プログラムの環境から資格情報を直接読み取ります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-138">[_Environment-based authentication_](#use-environment-based-authentication) reads credentials directly from the program's environment.</span></span>
* <span data-ttu-id="28e5f-139">[_ファイル ベースの認証_](#use-file-based-authentication)では、サービス プリンシパルの資格情報が含まれたファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-139">[_File-based authentication_](#use-file-based-authentication) loads a file containing service principal credentials.</span></span>
* <span data-ttu-id="28e5f-140">"[_クライアント ベースの認証_](#use-an-authentication-client)" では、コード内のオブジェクトを使用します。プログラムの実行時に資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-140">[_Client-based authentication_](#use-an-authentication-client) uses an object in code and makes you responsible for providing the credentials during program execution.</span></span>
* <span data-ttu-id="28e5f-141">"[_デバイス トークン認証_](#use-device-token-authentication)" では、ユーザーはトークンを使用して Web ブラウザーを介して対話形式でサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-141">[_Device token authentication_](#use-device-token-authentication) requires users to sign in interactively through a web browser with a token.</span></span>

<span data-ttu-id="28e5f-142">認証の関数と型はすべて `github.com/Azure/go-autorest/autorest/azure/auth` パッケージで使用できます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-142">All authentication functions and types are available in the `github.com/Azure/go-autorest/autorest/azure/auth` package.</span></span>

> [!NOTE]
> <span data-ttu-id="28e5f-143">特別な要件がない限り、クライアント ベースの認証は避けてください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-143">Unless you have special requirements, avoid client-based authentication.</span></span> <span data-ttu-id="28e5f-144">この認証方法では、バッド プラクティスが助長されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-144">This method of authentication encourages bad practices.</span></span> <span data-ttu-id="28e5f-145">特に、クライアント ベースの認証を使用すると、資格情報をハードコードしてしまいがちになります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-145">In particular, using client-based authentication makes it tempting to hard-code credentials.</span></span> <span data-ttu-id="28e5f-146">また、認証用のカスタム コードを記述すると、認証要件が変わった場合に、SDK の今後のリリースでコードが動作しなくなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-146">Writing custom code for authentication may also break under future SDK releases if authentication requirements change.</span></span>

## <a name="use-environment-based-authentication"></a><span data-ttu-id="28e5f-147">環境ベースの認証を使用する</span><span class="sxs-lookup"><span data-stu-id="28e5f-147">Use environment-based authentication</span></span>

<span data-ttu-id="28e5f-148">管理された設定でアプリケーションを実行している場合は、環境ベースの認証が自然な選択となります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-148">If you're running your application in a controlled setting, environment-based authentication is a natural choice.</span></span> <span data-ttu-id="28e5f-149">この認証方法を使用して、シェル環境を構成してからアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-149">With this authentication method, you configure the shell environment before running your application.</span></span> <span data-ttu-id="28e5f-150">実行時に、Azure で認証するために、これらの環境変数が Go SDK によって読み取られます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-150">At runtime, the Go SDK reads these environment variables to authenticate with Azure.</span></span>

<span data-ttu-id="28e5f-151">環境ベースの認証では、デバイス トークンを除くすべての認証方法がサポートされ、次の順番で評価されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-151">Environment-based authentication has support for all authentication methods except device tokens, evaluated in the following order:</span></span>

* <span data-ttu-id="28e5f-152">クライアントの資格情報</span><span class="sxs-lookup"><span data-stu-id="28e5f-152">Client credentials</span></span>
* <span data-ttu-id="28e5f-153">X509 証明書</span><span class="sxs-lookup"><span data-stu-id="28e5f-153">X509 certificates</span></span>
* <span data-ttu-id="28e5f-154">ユーザー名/パスワード</span><span class="sxs-lookup"><span data-stu-id="28e5f-154">Username/password</span></span>
* <span data-ttu-id="28e5f-155">Azure リソースのマネージド ID</span><span class="sxs-lookup"><span data-stu-id="28e5f-155">Managed identities for Azure resources</span></span>

<span data-ttu-id="28e5f-156">認証の種類に設定されていない値があったり、拒否されたりした場合は、SDK では自動的に次の認証の種類が試行されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-156">If an authentication type has unset values or is refused, the SDK automatically tries the next authentication type.</span></span> <span data-ttu-id="28e5f-157">試行できる種類がそれ以上ない場合、SDK によってエラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-157">When no more types are available to try, the SDK returns an error.</span></span>

<span data-ttu-id="28e5f-158">次の表に、環境ベースの認証でサポートされている各認証の種類で設定する必要がある環境変数の詳細を示します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-158">The following table details the environment variables that need to be set for each authentication type supported by environment-based authentication.</span></span>


|  <span data-ttu-id="28e5f-159">認証の種類</span><span class="sxs-lookup"><span data-stu-id="28e5f-159">Authentication type</span></span>   |     <span data-ttu-id="28e5f-160">環境変数</span><span class="sxs-lookup"><span data-stu-id="28e5f-160">Environment variable</span></span>     |                                                                                                     <span data-ttu-id="28e5f-161">説明</span><span class="sxs-lookup"><span data-stu-id="28e5f-161">Description</span></span>                                                                                                      |
|------------------------|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="28e5f-162">**クライアントの資格情報**</span><span class="sxs-lookup"><span data-stu-id="28e5f-162">**Client credentials**</span></span> |      `AZURE_TENANT_ID`       |                                                                    <span data-ttu-id="28e5f-163">サービス プリンシパルが属する Active Directory テナントの ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-163">The ID for the Active Directory tenant that the service principal belongs to.</span></span>                                                                     |
|                        |      `AZURE_CLIENT_ID`       |                                                                                       <span data-ttu-id="28e5f-164">サービス プリンシパルの名前または ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-164">The name or ID of the service principal.</span></span>                                                                                       |
|                        |    `AZURE_CLIENT_SECRET`     |                                                                                  <span data-ttu-id="28e5f-165">サービス プリンシパルに関連付けられているシークレット。</span><span class="sxs-lookup"><span data-stu-id="28e5f-165">The secret associated with the service principal.</span></span>                                                                                   |
|    <span data-ttu-id="28e5f-166">**証明書**</span><span class="sxs-lookup"><span data-stu-id="28e5f-166">**Certificate**</span></span>     |      `AZURE_TENANT_ID`       |                                                                   <span data-ttu-id="28e5f-167">証明書が登録されている Active Directory テナントの ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-167">The ID for the Active Directory tenant that the certificate is registered with.</span></span>                                                                    |
|                        |      `AZURE_CLIENT_ID`       |                                                                              <span data-ttu-id="28e5f-168">証明書に関連付けられているアプリケーション クライアント ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-168">The application client ID associated with the certificate.</span></span>                                                                              |
|                        |   `AZURE_CERTIFICATE_PATH`   |                                                                                       <span data-ttu-id="28e5f-169">クライアント証明書ファイルのパス。</span><span class="sxs-lookup"><span data-stu-id="28e5f-169">The path to the client certificate file.</span></span>                                                                                       |
|                        | `AZURE_CERTIFICATE_PASSWORD` |                                                                                       <span data-ttu-id="28e5f-170">クライアント証明書のパスワード。</span><span class="sxs-lookup"><span data-stu-id="28e5f-170">The password for the client certificate.</span></span>                                                                                       |
| <span data-ttu-id="28e5f-171">**ユーザー名/パスワード**</span><span class="sxs-lookup"><span data-stu-id="28e5f-171">**Username/Password**</span></span>  |      `AZURE_TENANT_ID`       |                                                                           <span data-ttu-id="28e5f-172">ユーザーが属する Active Directory テナントの ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-172">The ID for the Active Directory tenant that the user belongs to.</span></span>                                                                           |
|                        |      `AZURE_CLIENT_ID`       |                                                                                              <span data-ttu-id="28e5f-173">アプリケーション クライアント ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-173">The application client ID.</span></span>                                                                                              |
|                        |       `AZURE_USERNAME`       |                                                                                            <span data-ttu-id="28e5f-174">サインインに使用するユーザー名。</span><span class="sxs-lookup"><span data-stu-id="28e5f-174">The username to sign in with.</span></span>                                                                                             |
|                        |       `AZURE_PASSWORD`       |                                                                                            <span data-ttu-id="28e5f-175">サインインに使用するパスワード。</span><span class="sxs-lookup"><span data-stu-id="28e5f-175">The password to sign in with.</span></span>                                                                                             |
|  <span data-ttu-id="28e5f-176">**管理対象 ID**</span><span class="sxs-lookup"><span data-stu-id="28e5f-176">**Managed identity**</span></span>  |                              | <span data-ttu-id="28e5f-177">マネージド ID 認証では資格情報は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="28e5f-177">No credentials are needed for managed identity authentication.</span></span> <span data-ttu-id="28e5f-178">アプリケーションは、マネージド ID を使用するように構成された Azure リソース上で実行されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-178">The application must be running on an Azure resource configured to use managed identities.</span></span> <span data-ttu-id="28e5f-179">詳細については、[Azure リソースのマネージド ID]に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-179">For details, see [Managed identities for Azure resources].</span></span> |

<span data-ttu-id="28e5f-180">既定の Azure パブリック クラウド以外のクラウドまたは管理エンドポイントに接続するには、次の環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-180">To connect to a cloud or management endpoint other than the default Azure public cloud, set the following environment variables.</span></span> <span data-ttu-id="28e5f-181">Azure Stack、異なる地理的リージョンのクラウド、またはクラシック デプロイ モデルを使用する場合が最も一般的な理由です。</span><span class="sxs-lookup"><span data-stu-id="28e5f-181">The most common reasons are if you use Azure Stack, a cloud in a different geographic region, or the classic deployment model.</span></span>

| <span data-ttu-id="28e5f-182">環境変数</span><span class="sxs-lookup"><span data-stu-id="28e5f-182">Environment variable</span></span> | <span data-ttu-id="28e5f-183">説明</span><span class="sxs-lookup"><span data-stu-id="28e5f-183">Description</span></span>  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | <span data-ttu-id="28e5f-184">接続先のクラウド環境の名前。</span><span class="sxs-lookup"><span data-stu-id="28e5f-184">The name of the cloud environment to connect to.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="28e5f-185">接続するときに管理エンドポイントの URI として使用する Active Directory リソース ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-185">The Active Directory resource ID to use when connecting, as a URI to your management endpoint.</span></span> |

<span data-ttu-id="28e5f-186">環境ベースの認証を使用する場合は、[NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) 関数を呼び出して authorizer オブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-186">When using environment-based authentication, call the [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) function to get your authorizer object.</span></span> <span data-ttu-id="28e5f-187">その後、このオブジェクトがクライアントの `Authorizer` プロパティで設定され、Azure へのクライアントのアクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-187">This object is then set on the `Authorizer` property of clients to allow them access to Azure.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a><span data-ttu-id="28e5f-188">Azure Stack での認証</span><span class="sxs-lookup"><span data-stu-id="28e5f-188">Authentication on Azure Stack</span></span>

<span data-ttu-id="28e5f-189">Azure Stack で認証するには、次の変数を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-189">To authenticate on Azure Stack, you need to set the following variables:</span></span>

| <span data-ttu-id="28e5f-190">環境変数</span><span class="sxs-lookup"><span data-stu-id="28e5f-190">Environment variable</span></span> | <span data-ttu-id="28e5f-191">説明</span><span class="sxs-lookup"><span data-stu-id="28e5f-191">Description</span></span>  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | <span data-ttu-id="28e5f-192">Active Directory エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="28e5f-192">The Active Directory endpoint.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="28e5f-193">Active Directory リソース ID。</span><span class="sxs-lookup"><span data-stu-id="28e5f-193">The Active Directory resource ID.</span></span> |

<span data-ttu-id="28e5f-194">これらの変数は、Azure Stack のメタデータ情報から取得できます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-194">These variables can be retrieved from Azure Stack metadata information.</span></span> <span data-ttu-id="28e5f-195">メタデータを取得するには、Azure Stack 環境で Web ブラウザーを開き、URL として `(ResourceManagerURL)/metadata/endpoints?api-version=1.0` を使用します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-195">To retrieve the metadata, open a web browser in your Azure Stack environment and use the url: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`</span></span>

<span data-ttu-id="28e5f-196">`ResourceManagerURL` は、Azure Stack デプロイのリージョン名、マシン名、外部完全修飾ドメイン名 (FQDN) によって異なります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-196">The `ResourceManagerURL` varies based on the region name, machine name, and external fully qualified domain name (FQDN) of your Azure Stack deployment:</span></span>

| <span data-ttu-id="28e5f-197">環境</span><span class="sxs-lookup"><span data-stu-id="28e5f-197">Environment</span></span> | <span data-ttu-id="28e5f-198">ResourceManagerURL</span><span class="sxs-lookup"><span data-stu-id="28e5f-198">ResourceManagerURL</span></span> |
|----------------------|--------------|
| <span data-ttu-id="28e5f-199">開発キット</span><span class="sxs-lookup"><span data-stu-id="28e5f-199">Development Kit</span></span> | `https://management.local.azurestack.external/` |
| <span data-ttu-id="28e5f-200">統合システム</span><span class="sxs-lookup"><span data-stu-id="28e5f-200">Integrated Systems</span></span> | `https://management.(region).ext-(machine-name).(FQDN)` |

<span data-ttu-id="28e5f-201">Azure Stack での Azure SDK for Go の使用方法の詳細については、「[Azure Stack での GO による API バージョンのプロファイルの使用](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-201">For more information on how to use the Azure SDK for Go on Azure Stack, see [Use API version profiles with Go in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)</span></span>

## <a name="use-file-based-authentication"></a><span data-ttu-id="28e5f-202">ファイル ベースの認証を使用する</span><span class="sxs-lookup"><span data-stu-id="28e5f-202">Use file-based authentication</span></span>

<span data-ttu-id="28e5f-203">ファイルベースの認証では、[Azure CLI](/cli/azure) によって生成されるファイル形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-203">File-based authentication uses a file format generated by [the Azure CLI](/cli/azure).</span></span> <span data-ttu-id="28e5f-204">`--sdk-auth` パラメーターを指定して新しいサービス プリンシパルを作成すると、このファイルを簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-204">You can easily create this file when creating a new service principal with the `--sdk-auth` parameter.</span></span> <span data-ttu-id="28e5f-205">ファイル ベースの認証を使用する場合は、サービス プリンシパルの作成時に、この引数が指定されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-205">If you plan on using file-based authentication, make sure that this argument is provided when creating a service principal.</span></span> <span data-ttu-id="28e5f-206">CLI では出力が `stdout` に生成されるので、出力をファイルにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="28e5f-206">Since the CLI prints output to `stdout`, redirect output to a file.</span></span>

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

<span data-ttu-id="28e5f-207">`AZURE_AUTH_LOCATION` 環境変数を、承認ファイルがある場所に設定します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-207">Set the `AZURE_AUTH_LOCATION` environment variable to where the authorization file is located.</span></span> <span data-ttu-id="28e5f-208">この環境変数をアプリケーションが読み取り、環境変数内の資格情報が解析されます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-208">This environment variable is read by the application, and the credentials within it are parsed.</span></span> <span data-ttu-id="28e5f-209">承認ファイルを実行時に選択する必要がある場合は、[os.Setenv](https://golang.org/pkg/os/#Setenv) 関数を使用してプログラムの環境を操作します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-209">If you need to select the authorization file at runtime, manipulate the program's environment using the [os.Setenv](https://golang.org/pkg/os/#Setenv) function.</span></span>

<span data-ttu-id="28e5f-210">認証情報を読み込むには、[NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-210">To load the authentication information, call the [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) function.</span></span> <span data-ttu-id="28e5f-211">環境ベースの承認とは異なり、ファイル ベースの承認にはリソース エンドポイントが必要です。</span><span class="sxs-lookup"><span data-stu-id="28e5f-211">Unlike environment-based authorization, file-based authorization requires a resource endpoint.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

<span data-ttu-id="28e5f-212">サービス プリンシパルの使用とアクセス許可の管理の詳細については、[Azure CLI でのサービス プリンシパルの作成]に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-212">For more on using service principals and managing their access permissions, see [Create a service principal with Azure CLI].</span></span>

## <a name="use-device-token-authentication"></a><span data-ttu-id="28e5f-213">デバイス トークン認証を使用する</span><span class="sxs-lookup"><span data-stu-id="28e5f-213">Use device token authentication</span></span>

<span data-ttu-id="28e5f-214">ユーザーが対話形式でサインインできるようにする場合、最良の方法は、デバイス トークン認証を使用することです。</span><span class="sxs-lookup"><span data-stu-id="28e5f-214">If you want users to sign in interactively, the best way is through device token authentication.</span></span> <span data-ttu-id="28e5f-215">この認証フローでは、Microsoft サインイン サイトに貼り付けるトークンをユーザーに渡し、ユーザーはこのサイトで Azure Active Directory (AAD) アカウントで認証します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-215">This authentication flow passes the user a token to paste into a Microsoft sign-in site, where they then authenticate with an Azure Active Directory (AAD) account.</span></span> <span data-ttu-id="28e5f-216">標準のユーザー名/パスワード認証とは異なり、この認証方法では多要素認証が有効になっているアカウントがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-216">This authentication method supports accounts that have multi-factor authentication enabled, unlike standard username/password authentication.</span></span>

<span data-ttu-id="28e5f-217">デバイス トークン認証を使用するには、[NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) 関数で [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) Authorizer を作成します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-217">To use device token authentication, create a [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) authorizer with the [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) function.</span></span> <span data-ttu-id="28e5f-218">作成されたオブジェクトで [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) を呼び出して認証プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-218">Call [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) on the resulting object to start the authentication process.</span></span> <span data-ttu-id="28e5f-219">デバイス認証フローでは、認証フロー全体が完了するまでプログラムの実行がブロックされます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-219">Device flow authentication blocks program execution until the whole authentication flow is complete.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a><span data-ttu-id="28e5f-220">認証クライアントを使用する</span><span class="sxs-lookup"><span data-stu-id="28e5f-220">Use an authentication client</span></span>

<span data-ttu-id="28e5f-221">特定の種類の認証が必要であり、ユーザーの認証情報を読み込む処理をプログラムで実行する場合は、[auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) インターフェイスに準拠したクライアントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-221">If you require a specific type of authentication and are willing to have your program do the work to load authentication information from the user, you can use any client that conforms to the [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) interface.</span></span> <span data-ttu-id="28e5f-222">次の場合にこのインターフェイスを実装する種類を使用します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-222">Use a type that implements this interface when you:</span></span>

* <span data-ttu-id="28e5f-223">対話型プログラムを記述する</span><span class="sxs-lookup"><span data-stu-id="28e5f-223">Write an interactive program</span></span>
* <span data-ttu-id="28e5f-224">特別な構成ファイルを使用する</span><span class="sxs-lookup"><span data-stu-id="28e5f-224">Use specialized configuration files</span></span>
* <span data-ttu-id="28e5f-225">組み込みの認証方法を使用できないようにする要件がある</span><span class="sxs-lookup"><span data-stu-id="28e5f-225">Have a requirement that prevents using a built-in authentication method</span></span>

> [!WARNING]
> <span data-ttu-id="28e5f-226">Azure の資格情報をアプリケーションにハードコードしないでください。</span><span class="sxs-lookup"><span data-stu-id="28e5f-226">Never hard-code Azure credentials into an application.</span></span> <span data-ttu-id="28e5f-227">シークレットをアプリケーションのバイナリに配置すると、アプリケーションが実行されているかどうかに関係なく、攻撃者がシークレットを簡単に抽出できるようになります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-227">Putting secrets into an application binary makes it easier for an attacker to extract them, whether the application is running or not.</span></span> <span data-ttu-id="28e5f-228">この場合、資格情報が承認されているすべての Azure リソースが危険にさらされます。</span><span class="sxs-lookup"><span data-stu-id="28e5f-228">This puts all Azure resources the credentials are authorized for at risk!</span></span>

<span data-ttu-id="28e5f-229">次の表に、`AuthorizerConfig` インターフェイスに準拠した SDK の型を示します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-229">The following table lists the types in the SDK that conform to the `AuthorizerConfig` interface.</span></span>

| <span data-ttu-id="28e5f-230">認証の種類</span><span class="sxs-lookup"><span data-stu-id="28e5f-230">Authentication type</span></span> | <span data-ttu-id="28e5f-231">Authorizer の型</span><span class="sxs-lookup"><span data-stu-id="28e5f-231">Authorizer type</span></span> |
|---------------------|-----------------------|
| <span data-ttu-id="28e5f-232">証明書ベースの認証</span><span class="sxs-lookup"><span data-stu-id="28e5f-232">Certificate-based authentication</span></span> | <span data-ttu-id="28e5f-233">[ClientCertificateConfig]</span><span class="sxs-lookup"><span data-stu-id="28e5f-233">[ClientCertificateConfig]</span></span> |
| <span data-ttu-id="28e5f-234">クライアントの資格情報</span><span class="sxs-lookup"><span data-stu-id="28e5f-234">Client credentials</span></span> | <span data-ttu-id="28e5f-235">[ClientCredentialsConfig]</span><span class="sxs-lookup"><span data-stu-id="28e5f-235">[ClientCredentialsConfig]</span></span> |
| <span data-ttu-id="28e5f-236">Azure リソースのマネージド ID</span><span class="sxs-lookup"><span data-stu-id="28e5f-236">Managed identities for Azure resources</span></span> | <span data-ttu-id="28e5f-237">[MSIConfig]</span><span class="sxs-lookup"><span data-stu-id="28e5f-237">[MSIConfig]</span></span> |
| <span data-ttu-id="28e5f-238">ユーザー名/パスワード</span><span class="sxs-lookup"><span data-stu-id="28e5f-238">Username/password</span></span> | <span data-ttu-id="28e5f-239">[UsernamePasswordConfig]</span><span class="sxs-lookup"><span data-stu-id="28e5f-239">[UsernamePasswordConfig]</span></span> |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

<span data-ttu-id="28e5f-244">関連付けられた `New` 関数で認証子を作成し、作成されたオブジェクトで `Authorize` を呼び出して認証します。</span><span class="sxs-lookup"><span data-stu-id="28e5f-244">Create an authenticator with its associated `New` function, and then call `Authorize` on the resulting object to authenticate.</span></span> <span data-ttu-id="28e5f-245">たとえば、証明書ベースの認証を使用する場合は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="28e5f-245">For example, to use certificate-based authentication:</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
