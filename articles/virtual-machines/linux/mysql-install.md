---
title: Configurar o MySQL em uma VM do Linux no Azure | Microsoft Docs
description: Saiba como instalar a pilha do MySQL em uma máquina virtual do Linux (sistema operacional da família Ubuntu ou Red Hat) no Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: 21ad3f9baf4b8e117f881d9a36fc606af04e17a5
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158432"
---
# <a name="how-to-install-mysql-on-azure"></a>Como instalar o MySQL no Azure
Neste artigo, você aprenderá a instalar e configurar o MySQL em uma máquina virtual do Azure que executa o Linux.


> [!NOTE]
> Você já deve ter uma máquina virtual do Microsoft Azure executando o Linux para concluir este tutorial. Consulte o [Tutorial de VM Linux do Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para criar e configurar uma VM Linux com `mysqlnode` como o nome da VM e `azureuser` como usuário antes de continuar.
> 
> 

Nesse caso, use a porta 3306 como a porta do MySQL.  

Usaremos o pacote do repositório para instalar o MySQL5.6 como um exemplo neste artigo. Na verdade, o MySQL5.6 demonstra melhoria de desempenho superior ao MySQL5.5.  Mais informações [aqui](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

## <a name="install-mysql56-on-ubuntu"></a>Instalar o MySQL5.6 no Ubuntu
Usaremos uma VM do Linux executando o Ubuntu.


### <a name="install-mysql"></a>Instalar MySQL

Instalar o MySQL Server 5.6 alternar para o `root` usuário:

```bash  
sudo su -
```

Instalar o mysql-server 5.6:

```bash  
apt-get update
apt-get -y install mysql-server-5.6
```

  
Durante a instalação, você verá uma janela de diálogo parece que você defina a senha raiz do MySQL abaixo e você precisa definir a senha aqui.
  
![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

Insira a senha novamente para confirmar.

![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

### <a name="sign-in"></a>Entrar
  
Quando terminar a instalação do MySQL Server, o serviço do MySQL será iniciado automaticamente. Você pode entrar no servidor MySQL com o `root` usuário e digite sua senha.

```bash  
mysql -uroot -p
```


### <a name="manage-the-mysql-service"></a>Gerenciar o serviço MySQL

Obter o status do serviço MySQL

```bash   
service mysql status
```
  
Iniciar o serviço MySQL

```bash  
service mysql start
```
  
Parar o serviço MySQL

```bash  
service mysql stop
```
  
Reinicie o serviço MySQL

```bash  
service mysql restart
```

## <a name="install-mysql-on-red-hat-os-centos-oracle-linux"></a>Instalar o MySQL no sistema operacional de Red Hat, CentOS, Oracle Linux
Usaremos uma VM do Linux com CentOS ou Oracle Linux aqui.

### <a name="add-the-mysql-yum-repository"></a>Adicionar repositório MySQL yum
    
Alterne para `root` usuário:

```bash  
sudo su -
```

Baixe e instale o pacote de versão do MySQL:

```bash  
wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
yum localinstall -y mysql-community-release-el6-5.noarch.rpm
```

### <a name="enable-the-mysql-repository"></a>Habilitar o repositório de MySQL
edite o arquivo abaixo para habilitar o repositório do MySQL para baixar o pacote MySQL5.6.

```bash  
vim /etc/yum.repos.d/mysql-community.repo
```

  
Atualize cada valor deste arquivo conforme indicado abaixo:

```  
\# *Enable to use MySQL 5.6*
  
[mysql56-community]
name=MySQL 5.6 Community Server
  
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
enabled=1
  
gpgcheck=1
  
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

### <a name="install-mysql"></a>Instalar MySQL 

Instale o MySQL do repositório.

```bash  
yum install mysql-community-server
```
  
O pacote RPM MySQL e todos os pacotes relacionados serão instalados.


## <a name="manage-the-mysql-service"></a>Gerenciar o serviço MySQL
  
Verifique o status do serviço do MySQL server:

```bash  
service mysqld status\
```
  
Verifique se a porta padrão do MySQL server está em execução:

```bash  
netstat  –tunlp|grep 3306
```

Inicie o servidor MySQL:

```bash
service mysqld start
```

Pare o MySQL server:

```bash
service mysqld stop
```

Defina o MySQL para iniciar durante a inicialização do sistema para:

```bash
chkconfig mysqld on
```

## <a name="install-mysql-on-suse-linux"></a>Instalar o MySQL no SUSE Linux

Usaremos a VM do Linux com OpenSUSE aqui.

### <a name="download-and-install-mysql-server"></a>baixar e instalar o MySQL Server
  
Alterne para o usuário `root` com o comando abaixo:  

```bash  
sudo su -
```
  
Baixe e instale o pacote de MySQL:

```bash  
zypper update
zypper install mysql-server mysql-devel mysql
```

### <a name="manage-the-mysql-service"></a>Gerenciar o serviço MySQL
  
Verificar o status do servidor MySQL:

```bash  
rcmysql status
```
  
Verifique se a porta padrão do MySQL server:

```bash  
netstat  –tunlp|grep 3306
```

Inicie o servidor MySQL:

```bash
rcmysql start
```

Pare o MySQL server:

```bash
rcmysql stop
```

Defina o MySQL para iniciar durante a inicialização do sistema para:

```bash
insserv mysql
```

## <a name="next-step"></a>Próxima etapa
Para obter mais informações, consulte o [MySQL](https://www.mysql.com/) site.

