---
title: Proteger serviços web usando SSL
titleSuffix: Azure Machine Learning service
description: Saiba como proteger um serviço web que é implantado por meio do serviço do Azure Machine Learning, habilitando o HTTPS. HTTPS protege os dados pelos clientes, usando o protocolo TLS (TLS), uma substituição para camadas de soquete seguro (SSL). Os clientes também usam HTTPS para verificar a identidade do serviço web.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 779a34800057284ce77b6d76e030ddca5fadc25f
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393818"
---
# <a name="use-ssl-to-secure-a-web-service-through-azure-machine-learning"></a>Usar SSL para proteger um serviço web por meio do Azure Machine Learning

Este artigo mostra como proteger um serviço web que é implantado por meio do serviço do Azure Machine Learning.

Você usa [HTTPS](https://en.wikipedia.org/wiki/HTTPS) para restringir o acesso a serviços da web e proteger os dados que os clientes enviam. HTTPS ajuda a proteger as comunicações entre um cliente e um serviço web ao criptografar as comunicações entre os dois. A criptografia usa [segurança de camada de transporte (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS, às vezes, ainda é conhecido como *Secure Sockets Layer* (SSL), que foi o predecessor do TLS.

> [!TIP]
> O SDK do Azure Machine Learning usa o termo "SSL" para propriedades que são relacionadas a comunicações seguras. Isso não significa que o serviço web não usa *TLS*. O SSL é apenas um termo mais comumente reconhecido.

TLS e SSL ambos dependem *certificados digitais*, que pode ajudar com a verificação de identidade e criptografia. Para obter mais informações sobre certificados digitais como funcionam, consulte o tópico da Wikipedia [infraestrutura de chave pública](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!WARNING]
> Se você não usar HTTPS para o serviço web, os dados que são enviados de e para o serviço podem ser visíveis para outras pessoas na internet.
>
> HTTPS também permite que o cliente verificar a autenticidade do servidor que ele está se conectando. Esse recurso protege os clientes contra [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) ataques.

Este é o processo geral para proteger um serviço web:

1. Obter um nome de domínio.

2. Obtenha um certificado digital.

3. Implantar ou atualizar o serviço web com SSL habilitado.

4. Atualizar o DNS para apontar para o serviço Web.

> [!IMPORTANT]
> Se você estiver implantando para o serviço de Kubernetes do Azure (AKS), você pode comprar seu próprio certificado ou usar um certificado que é fornecido pela Microsoft. Se você usar um certificado da Microsoft, você não precisa obter um nome de domínio ou um certificado SSL. Para obter mais informações, consulte o [habilitar o SSL e implantar](#enable) seção deste artigo.

Há pequenas diferenças ao proteger serviços web em toda [destinos de implantação](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Obter um nome de domínio

Se você ainda não tem um nome de domínio, adquirir um de um *registrador de nome de domínio*. O processo e os preços diferem entre registradores. O registrador fornece ferramentas para gerenciar o nome de domínio. Usar essas ferramentas para mapear um nome de domínio totalmente qualificado (FQDN) (como www\.contoso.com) para o endereço IP que hospeda o serviço web.

## <a name="get-an-ssl-certificate"></a>Obter um certificado SSL

Há várias maneiras de obter um certificado SSL (certificado digital). O mais comum é comprar um de uma *autoridade de certificação* (CA). Independentemente de onde você pode obter o certificado, você precisa ter os seguintes arquivos:

* Um **certificado**. O certificado deve conter a cadeia de certificados completa, e deve ser "PEM codificado."
* Uma **chave**. A chave também deve ser PEM codificado.

Quando você solicita um certificado, você deve fornecer o FQDN do endereço que você planeja usar para o serviço web (por exemplo, www\.contoso.com). O endereço que está identificado no certificado e o endereço que os clientes usam são comparados para verificar a identidade do serviço web. Se esses endereços não corresponderem, o cliente recebe uma mensagem de erro.

> [!TIP]
> Se a autoridade de certificação não é possível fornecer o certificado e a chave como arquivos PEM codificado, você pode usar um utilitário, como [OpenSSL](https://www.openssl.org/) para alterar o formato.

> [!WARNING]
> Use *autoassinados* certificados apenas para desenvolvimento. Não usá-los em ambientes de produção. Certificados autoassinados podem causar problemas nos aplicativos clientes. Para obter mais informações, consulte a documentação para as bibliotecas de rede que usa o aplicativo cliente.

## <a id="enable"></a> Habilitar o SSL e implantar

Para implantar (ou reimplantar) o serviço com SSL habilitado, defina as *ssl_enabled* parâmetro como "True", onde quer que ele é aplicável. Defina a *ssl_certificate* parâmetro para o valor da *certificado* arquivo. Defina a *ssl_key* para o valor da *chave* arquivo.

### <a name="deploy-on-aks-and-field-programmable-gate-array-fpga"></a>Implantar no AKS e portões programáveis em campo FPGA (matriz)

  > [!NOTE]
  > As informações nesta seção também se aplica quando você implanta um serviço web seguro para a interface visual. Se você não estiver familiarizado com o usando o SDK do Python, consulte [o que é o SDK de aprendizado de máquina do Azure para Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

Quando você implanta em AKS, você pode criar um novo cluster do AKS ou anexará um existente.
  
-  Se você criar um novo cluster, você usa  **[AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none-)** .
- Se você anexar a um cluster existente, você usa  **[AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none-)** . Ambos retornam um objeto de configuração que tem um **enable_ssl** método.

O **enable_ssl** método pode usar um certificado que é fornecido pela Microsoft ou um certificado que você compra.

  * Quando você usa um certificado da Microsoft, você deve usar o *leaf_domain_label* parâmetro. Esse parâmetro gera o nome DNS para o serviço. Por exemplo, um valor de "myservice" cria um nome de domínio de "myservice\<seis aleatório caracteres >.\< azureregion >. cloudapp.azure.com ", onde \<azureregion > é a região que contém o serviço. Opcionalmente, você pode usar o *overwrite_existing_domain* parâmetro para substituir o existente *leaf_domain_label*.

    Para implantar (ou reimplantar) o serviço com SSL habilitado, defina as *ssl_enabled* parâmetro como "True", onde quer que ele é aplicável. Defina a *ssl_certificate* parâmetro para o valor da *certificado* arquivo. Defina a *ssl_key* para o valor da *chave* arquivo.

    > [!IMPORTANT]
    > Quando você usa um certificado da Microsoft, você não precisa comprar seu próprio certificado ou nome de domínio.

    O exemplo a seguir demonstra como criar uma configuração que permite que um certificado SSL da Microsoft:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(leaf_domain_label = "myservice")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                          cluster_name = cluster_name)
    attach_config.enable_ssl(leaf_domain_label = "myservice")
    ```

  * Quando você usa *um certificado que você adquiriu*, você usa o *ssl_cert_pem_file*, *ssl_key_pem_file*, e *ssl_cname* parâmetros. O exemplo a seguir demonstra como usar *. PEM* arquivos para criar uma configuração que usa um certificado SSL que você comprou:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
    attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

Para obter mais informações sobre *enable_ssl*, consulte [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) e [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

### <a name="deploy-on-azure-container-instances"></a>Implantar em instâncias de contêiner do Azure

Quando você implanta instâncias de contêiner do Azure, forneça valores para os parâmetros relacionados ao SSL, como mostra o seguinte trecho de código:

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
```

Para obter mais informações, consulte [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none-).

## <a name="update-your-dns"></a>Atualizar o DNS

Em seguida, você precisa atualizar o DNS para apontar para o serviço Web.

+ **Para instâncias de contêiner:**

  Use as ferramentas do seu registrador de nome de domínio para atualizar o registro DNS para seu nome de domínio. O registro deve apontar para o endereço IP do serviço.

  Pode haver um atraso de minutos ou horas antes que os clientes podem resolver o nome de domínio, dependendo do registrador e a "vida útil" (TTL) que está configurado para o nome de domínio.

+ **Para o AKS:**

  > [!WARNING]
  > Se você usou *leaf_domain_label* para criar o serviço usando um certificado da Microsoft, não atualize manualmente o valor DNS para o cluster. O valor deve ser definido automaticamente.

  Atualizar o DNS na **configuração** guia do endereço IP público do cluster AKS. (Consulte a imagem a seguir). O endereço IP público é um tipo de recurso é criado sob o grupo de recursos que contém os nós de agente do AKS e outros recursos de rede.

  ![Serviço do Azure Machine Learning: Proteger os serviços Web com SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Próximas etapas
Saiba como:
+ [Consumir um modelo de machine learning implantado como um serviço web](how-to-consume-web-service.md)
+ [Executar com segurança experimentos e Inferência de tipos dentro de uma rede virtual do Azure](how-to-enable-virtual-network.md)
