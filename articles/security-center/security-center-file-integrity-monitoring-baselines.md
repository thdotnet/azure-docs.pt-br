---
title: Comparar linhas de base com o monitoramento de integridade de arquivo na Central de segurança do Azure | Microsoft Docs
description: Saiba como comparar linhas de base com o monitoramento de integridade de arquivo na Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c8a2a589-b737-46c1-b508-7ea52e301e8f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: monhaber
ms.openlocfilehash: e403a9bd4d3f8668544dab1d81e9052b37839bef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358432"
---
# <a name="compare-baselines-using-file-integrity-monitoring-fim"></a>Comparar linhas de base usando o monitoramento de integridade de arquivo (FIM)

Monitoramento de integridade de arquivo (FIM) informa quando ocorrerem alterações em áreas restritas em seus recursos, para que você possa investigar e atividades não autorizadas de endereço. FIM monitora os arquivos do Windows, Windows registros e arquivos do Linux.

Este tópico explica como habilitar a FIM de nos arquivos e registros. Para obter mais informações sobre o FIM, consulte [monitoramento de integridade de arquivo na Central de segurança do Azure](security-center-file-integrity-monitoring.md).

## <a name="why-use-fim"></a>Por que usar o FIM?

O estado de segurança e o comportamento de seus recursos de controle do sistema operacional, aplicativos e configurações associadas. Portanto, os invasores voltados para os arquivos que controlam os recursos, a fim de superar o sistema operacional de um recurso e/ou executar atividades sem ser detectado.

Na verdade, muitos padrões de conformidade a normas, como a PCI-DSS e ISO 17799 exigem a implementação de controles FIM.  

## <a name="enable-built-in-recursive-registry-checks"></a>Habilitar as verificações de registro interno recursiva

Os padrões de hive de registro do FIM fornecem uma maneira conveniente para monitorar as alterações de recursiva dentro de áreas de segurança comuns.  Por exemplo, um adversário pode configurar um script para ser executado no contexto LOCAL_SYSTEM Configurando uma execução na inicialização ou desligamento.  Para monitorar as alterações desse tipo, habilite a verificação interna.  

![Registro](./media/security-center-file-integrity-monitoring-baselines/baselines-registry.png)

>[!NOTE]
> Verificações de recursiva se aplicam somente às seções de segurança recomendadas e não a caminhos de registro personalizado.  

## <a name="adding-a-custom-registry-check"></a>Adicionando uma verificação do registro personalizada

Linhas de base do FIM comece identificando as características de um estado válido para o sistema operacional e oferecer suporte a aplicativo.  Neste exemplo, nos concentraremos nas configurações de política de senha para o Windows Server 2008 e superior.


|Nome da política                 | Configuração do registro|
|---------------------------------------|-------------|
|Controlador de domínio: Recusar alterações de senha de conta de computador| MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RefusePasswordChange|
|Membro do domínio: Criptografar ou assinar digitalmente os dados de canal seguro (sempre)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireSignOrSeal|
|Membro do domínio: Criptografar digitalmente os dados de canal seguro (quando possível)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\SealSecureChannel|
|Membro do domínio: Assinar digitalmente os dados de canal (quando possível) seguro|MACHINE\System\CurrentControlSet\Services   \Netlogon\Parameters\SignSecureChannel|
|Membro do domínio: Desabilitar alterações de senha de conta de máquina|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DisablePasswordChange|
|Membro do domínio: Duração máxima da máquina de senha de conta|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\MaximumPasswordAge|
|Membro do domínio: Requer uma chave de sessão forte (Windows 2000 ou posterior)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireStrongKey|
|Segurança de rede: Restringir NTLM:  Autenticação NTLM neste domínio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RestrictNTLMInDomain|
|Segurança de rede: Restringir NTLM: Adicionar exceções de servidor neste domínio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DCAllowedNTLMServers|
|Segurança de rede: Restringir NTLM: Auditar autenticação NTLM neste domínio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\AuditNTLMInDomain|

> [!NOTE]
> Para saber mais sobre as configurações do registro com suporte por várias versões de sistema operacional, consulte o [planilha de referência de configurações de diretiva de grupo](https://www.microsoft.com/en-us/download/confirmation.aspx?id=25250).

*Para configurar o FIM para monitorar linhas de base do registro:*

1. No **adicionar registro do Windows para controle de alterações** janela, no **chave de registro do Windows** texto, digite a chave do registro.

    <code>

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters
    </code>

      ![Habilitar o FIM em um registro](./media/security-center-file-integrity-monitoring-baselines/baselines-add-registry.png)

## <a name="tracking-changes-to-windows-files"></a>Controlar alterações em arquivos do Windows

1. No **adicionar o arquivo do Windows para controle de alterações** janela, no **Enter caminho** texto, digite a pasta que contém os arquivos que você deseja controlar. No exemplo na figura a seguir **aplicativo Web da Contoso** reside o d:\ unidade dentro de **ContosWebApp** estrutura de pastas.  
1. Crie uma entrada de arquivo personalizada do Windows fornecendo um nome de classe de configuração, permitindo a recursão e especificando a pasta superior com um sufixo de curinga (*).

    ![Habilitar o FIM em um arquivo](./media/security-center-file-integrity-monitoring-baselines/baselines-add-file.png)

## <a name="retrieving-change-data"></a>Recuperando dados de alteração

Monitoramento de integridade de dados residem dentro do Azure Log Analytics de arquivo / tabela ConfigurationChange definida.  

 1. Defina um intervalo de tempo para recuperar um resumo das alterações por recurso.
No exemplo a seguir, estamos recuperando todas as alterações nos últimos 14 dias nas categorias de registro e arquivos:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry', 'Files')

    > | summarize count() by Computer, ConfigChangeType

    </code>

1. Para exibir os detalhes das alterações do registro:

    1. Remova **arquivos** da **onde** cláusula, 
    1. Remova a linha de resumo e substituí-lo com uma cláusula de ordenação:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry')

    > | order by Computer, RegistryKey

    </code>

Relatórios podem ser exportados em CSV para arquivamento e/ou canalizadas a um relatório do Power BI.  

![Data FIM](./media/security-center-file-integrity-monitoring-baselines/baselines-data.png)