---
title: Criar e usar um par de chaves SSH para VMs Linux no Azure | Microsoft Docs
description: "Como criar e usar um par de chaves SSH pública e privada para VMs Linux no Azure para melhorar a segurança do processo de autenticação."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/07/2017
ms.author: iainfou
experimental: true
experiment_id: rasquill-ssh-20170308
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 796e3ac201657bc5d596a71f034629fa2ebde7c7
ms.lasthandoff: 03/10/2017


---

# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Como criar e usar um par de chaves SSH pública e privada para VMs Linux no Azure
Com um par de chaves SSH (secure shell), você pode criar VMs (máquinas virtuais) no Azure que usam chaves SSH para autenticação, eliminando a necessidade de senhas para fazer logon. Este artigo mostra como gerar e usar rapidamente um par de arquivos de chave pública e privada RSA do protocolo SSH versão 2 para VMs Linux. Para obter outros exemplos e etapas mais detalhados, por exemplo, para usar com o portal clássico, consulte [etapas detalhadas para criar pares de chave SSH certificados](virtual-machines-linux-create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Criar um par de chaves SSH
Use o comando `ssh-keygen` para criar arquivos de chave SSH pública e privada criados por padrão no diretório `~/.ssh`, mas especifique um local diferente e uma senha adicional (uma senha para acessar o arquivo de chave privada) quando receber a solicitação. Execute o seguinte comando a partir do shell Bash, respondendo aos prompts com suas próprias informações.

```bash
ssh-keygen -t rsa -b 2048 
```

## <a name="use-the-ssh-key-pair"></a>Usar o par de chave SSH
A chave pública que você coloca em sua VM Linux no Azure é armazenada por padrão em `~/.ssh/id_rsa.pub`, a menos que você altere o local quando a criar. Se você usar a [CLI do Azure 2.0](/cli/azure) para criar a VM, especifique o local dessa chave pública quando usar [az vm create](/cli/azure/vm#create) com a opção `--ssh-key-path`. Se você copiar e colar o conteúdo do arquivo de chave pública para usar no Portal do Azure ou em um modelo do Resource Manager, não copie um espaço em branco adicional. Por exemplo, se você usar o OS X, direcione o arquivo de chave pública (por padrão, **~/.ssh/id_rsa.pub**) para **pbcopy** para copiar o conteúdo (há outros programas Linux que fazem a mesma coisa, como `xclip`). 

Se você não estiver familiarizado com as chaves públicas SSH, veja sua chave pública executando `cat` da seguinte forma, substituindo `~/.ssh/id_rsa.pub` por seu próprio local de arquivo de chave pública:

```bash
cat ~/.ssh/id_rsa.pub
```

Com a chave pública em sua VM do Azure, conecte-se via SSH com sua VM usando o endereço IP ou nome DNS de sua VM (lembre-se de substituir `azureuser` e `myvm.westus.cloudapp.azure.com` abaixo pelo nome de usuário de administrador e o nome de domínio totalmente qualificado – ou endereço IP):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Se você forneceu uma senha quando criou o par de chaves, insira a senha quando receber a solicitação durante o processo de logon. (O servidor é adicionado à sua pasta `~/.ssh/known_hosts` e você não receberá uma solicitação para se conectar novamente até que a chave pública em sua VM do Azure mude ou o nome do servidor seja removido do `~/.ssh/known_hosts`.)

## <a name="next-steps"></a>Próximas etapas

As VMs criadas com chaves SSH são, por padrão, configuradas com as senhas desabilitadas, para tornar as tentativas de adivinhação por força bruta muito mais caras e difíceis. Este tópico descreve como criar um par de chaves SSH simples para uso rápido. Se você precisar de mais ajuda com a criação de seu par de chaves SSH, ou precisar de outros certificados, por exemplo, para uso com o portal clássico, confira [Etapas detalhadas para criar pares de chave SSH e certificados](virtual-machines-linux-create-ssh-keys-detailed.md).

Crie VMs que usam seu par de chaves SSH usando o Portal do Azure, CLI e modelos:

* [Criar uma VM Linux segura usando o Portal do Azure](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma VM Linux segura usando a CLI do Azure 2.0)](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma VM do Linux segura usando um modelo do Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

