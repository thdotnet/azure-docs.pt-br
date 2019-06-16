---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 926fb3e9a2c09d30da549330842d8b7e185674ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66171761"
---
Em **Adicionar vCenter**, especifique um nome amigável para o host vSphere ou servidor vCenter e então especifique o endereço IP ou o FQDN do servidor. Deixe a porta como 443, a menos que os servidores do VMware estejam configurados para escutar solicitações em uma porta diferente. Selecione a conta que deve se conectar ao VMware vCenter ou ao servidor vSphere ESXi. Clique em **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Se você estiver adicionando o servidor VMware vCenter ou host VMware vSphere com uma conta que não tem privilégios de administrador no servidor vCenter ou host, certifique-se de que a conta tem estes privilégios habilitados: Datacenter, repositório de dados, pasta, Host, rede, recursos, Máquina Virtual e vSphere Distributed Switch. Além disso, o servidor VMware vCenter precisa do privilégio de exibição do Armazenamento habilitado.
