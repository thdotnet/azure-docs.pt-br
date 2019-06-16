---
title: Usar o certificado SSL de cliente no código do aplicativo – Serviço de Aplicativo do Azure | Microsoft Docs
description: Saiba como usar certificados de cliente para se conectar a recursos remotos que necessitam deles.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ead1892062912840c9931ae60d11c90975ad26ac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475103"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Usar um certificado SSL no código do aplicativo no Serviço de Aplicativo do Azure

Este guia de instruções mostra como usar certificados públicos ou privados no código do aplicativo. Um exemplo do caso de uso é que seu aplicativo acessa um serviço externo que exige autenticação de certificado.

Essa abordagem ao uso de certificados em seu código utiliza o SSL funcionalidade no serviço de aplicativo que exige que seu aplicativo esteja na **básica** camada ou superior. Como alternativa, você pode [incluir o arquivo de certificado no seu repositório de aplicativo](#load-certificate-from-file), mas não é uma prática recomendada para certificados privados.

Ao permitir que o Serviço de Aplicativo gerencie os certificados SSL, é possível manter os certificados e o código do aplicativo separados e proteger seus dados confidenciais.

## <a name="upload-a-private-certificate"></a>Carregar um certificado privado

Antes de carregar um certificado privado, certifique-se [atende a todos os requisitos](app-service-web-tutorial-custom-ssl.md#prepare-a-private-certificate), exceto que ele não precisa ser configurado para autenticação do servidor.

Quando estiver pronto para carregar, execute o seguinte comando <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config ssl upload --name <app-name> --resource-group <resource-group-name> --certificate-file <path-to-PFX-file> --certificate-password <PFX-password> --query thumbprint
```

Copie a impressão digital do certificado e ver [disponibilizar o certificado](#make-the-certificate-accessible).

## <a name="upload-a-public-certificate"></a>Carregar um certificado público

Certificados públicos dá suporte a *. cer* formato. Para carregar um certificado público, o <a href="https://portal.azure.com" target="_blank">portal do Azure</a>e navegue até seu aplicativo.

Clique em **configurações de SSL** > **certificados públicos (. cer)**  > **carregar certificado público** na navegação à esquerda de seu aplicativo.

Na **nome**, digite um nome para o certificado. Na **arquivo de certificado CER**, selecione o arquivo CER.

Clique em **Carregar**.

![Carregar um certificado público](./media/app-service-web-ssl-cert-load/private-cert-upload.png)

Depois que o certificado é carregado, copie a impressão digital do certificado e ver [disponibilizar o certificado](#make-the-certificate-accessible).

## <a name="import-an-app-service-certificate"></a>Importar um certificado de serviço de aplicativo

Ver [comprar e configurar um certificado SSL para o serviço de aplicativo do Azure](web-sites-purchase-ssl-web-site.md).

Depois que o certificado é importado, copie a impressão digital do certificado e ver [disponibilizar o certificado](#make-the-certificate-accessible).

## <a name="make-the-certificate-accessible"></a>Disponibilizar o certificado

Para usar um certificado carregado ou importado no código do aplicativo, verifique sua impressão digital acessível com o `WEBSITE_LOAD_CERTIFICATES` configuração do aplicativo, executando o seguinte comando na <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Para disponibilizar todos os certificados, defina o valor como `*`.

> [!NOTE]
> Essa configuração coloca os certificados especificados na [User\My atual](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) repositório para a maioria dos tipos de preço, mas na **isolado** camada (ou seja, o aplicativo é executado em um [o ambiente do serviço de aplicativo](environment/intro.md)), ele coloca os certificados na [Machine\My Local](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) armazenar.
>

![Definir a configuração do aplicativo](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Ao terminar, clique em **Salvar**.

Os certificados configurados agora estão prontos para ser usado pelo seu código.

## <a name="load-the-certificate-in-code"></a>Carregar o certificado no código

Quando o certificado estiver disponível, será possível acessá-lo no código C# por meio da impressão digital do certificado. O código a seguir carrega um certificado com a impressão digital `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="load-certificate-from-file"></a>Certificado de carga do arquivo

Se você precisar carregar um arquivo de certificado do seu diretório de aplicativo, é melhor carregá-lo usando [FTPS](deploy-ftp.md) em vez de [Git](deploy-local-git.md), por exemplo. Você deve manter os dados confidenciais, como um certificado privado fora do controle do código-fonte.

Mesmo que você está carregando o arquivo diretamente no seu código .NET, a biblioteca ainda verifica se o perfil do usuário atual será carregado. Para carregar o perfil do usuário atual, defina as `WEBSITE_LOAD_USER_PROFILE` configuração de aplicativo com o seguinte comando em de <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Depois que essa configuração é definida, o seguinte C# exemplo carrega um certificado chamado `mycert.pfx` da `certs` diretório do repositório do seu aplicativo.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```
