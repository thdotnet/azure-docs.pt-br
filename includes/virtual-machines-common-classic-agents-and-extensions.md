



As Extensões de VM podem ajudá-lo a:

* Modificar os recursos de segurança e identidade, como redefinir os valores de conta e usar antimalware
* Iniciar, parar ou configurar o monitoramento e o diagnóstico
* Redefinir ou instalar os recursos de conectividade, como RDP e SSH
* Diagnosticar, monitorar e gerenciar suas VMs

Também há muitos outros recursos; novos recursos de Extensão de VM são lançados regularmente. Este artigo descreve os agentes de VM do Azure para Windows e Linux e como dar suporte à funcionalidade de Extensão de VM. Para ver uma lista de Extensões de VM por categoria de recurso, veja [Extensões de VM e recursos do Azure](../articles/virtual-machines/virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="azure-vm-agents-for-windows-and-linux"></a>Agentes de VM do Azure para Windows e Linux
O Agente de Máquinas Virtuais do Azure (Agente de VM) é um processo seguro e leve que instala, configura e remove as extensões de VM em instâncias de máquinas virtuais do Azure da Galeria de imagens e em instâncias de VM personalizadas que tenham o agente de VM instalado. O agente de VM atua como o serviço de controle local seguro para a sua VM do Azure. As extensões que o agente carrega fornecem recursos específicos para aumentar a sua produtividade usando a instância.

Há dois agentes de VM do Azure, um para máquinas virtuais do Windows e outro para VMs do Linux. Por padrão, o Agente de VM é instalado automaticamente ao criar uma VM na Galeria de Imagens, mas você também pode instalar o Agente de VM após a instância ser criada ou instalá-lo em uma imagem de VM personalizada que você carrega por conta própria.

> [!IMPORTANT]
> Esses Agentes de VM são serviços muito leves que permitem a administração segura de instâncias de máquina virtual. Pode haver casos em que você não deseja o agente de VM. Em caso afirmativo, certifique-se de criar VMs que não tenham o Agente de VM instalado. Embora o Agente de VM possa ser removido fisicamente, o comportamento de quaisquer extensões de VM na instância é indefinido. Como resultado, no momento, não há suporte para remover o Agente de VM quando ele é instalado.
> 
> 

O Agente de VM é habilitado nas seguintes situações:

* Quando você cria uma instância de uma VM usando o método de **Criação Rápida** no portal clássico do Azure ou usando o método de **Criação Personalizada** no Portal Clássico do Azure e verificando se a caixa de seleção **Instalar o Agente de VM** está marcada (como mostrado na imagem abaixo). Para saber mais, consulte [Como criar uma máquina virtual personalizada](../articles/virtual-machines/virtual-machines-windows-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  
  ![Caixa de seleção do Agente de VM](./media/virtual-machines-common-classic-agents-and-extensions/IC719409.png)
* Quando você cria uma instância de uma VM usando o cmdlet [New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx) ou [New-AzureQuickVM](https://msdn.microsoft.com/library/azure/dn495183.aspx). Você pode criar uma VM sem o Agente de VM instalado adicionando o parâmetro **– DisableGuestAgent** ao cmdlet [Add-AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) .
* Baixando e instalando manualmente o Agente de VM (a versão do Windows ou do Linux) em uma instância VM existente e definindo o valor de **ProvisionGuestAgent** como **true** usando o PowerShell ou uma chamada REST. (Se você não definir esse valor após instalar manualmente o Agente de VM, a adição do Agente de VM não será detectada corretamente.) O exemplo de código a seguir mostra como fazer isso usando o PowerShell quando os argumentos `$svc` e `$name` já foram determinados.
  
      $vm = Get-AzureVM –ServiceName $svc –Name $name
      $vm.VM.ProvisionGuestAgent = $TRUE
      Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc
* Criando uma imagem de VM com o Agente de VM instalado antes de carregá-la no Azure. Para uma VM do Windows, baixe o [arquivo .msi do Agente de VM do Windows](http://go.microsoft.com/fwlink/?LinkID=394789) e instale o Agente de VM. Para uma VM do Linux, ela será instalada por meio do repositório do GitHub localizado em <https://github.com/Azure/WALinuxAgent>. Para saber mais sobre como instalar o Agente de VM no Linux, consulte o [Guia do usuário do Agente de VM no Linux do Azure](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> No PaaS, o Agente de VM é chamado **WindowsAzureGuestAgent** e sempre está disponível na Web e nas VMs de Função de Trabalho. (Para saber mais, consulte [Arquitetura de funções do Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).) Agora, o Agente de VM para VMs de função pode adicionar extensões às VMs de serviço de nuvem, da mesma maneira que faz para máquinas virtuais persistentes. A maior diferença entre as extensões de VM nas máquinas virtuais de função e nas máquinas virtuais persistentes é que com VMs de função, as extensões são adicionadas ao serviço de nuvem primeiro e, em seguida, às implantações no serviço em nuvem.
> 
> Use o cmdlet [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) para listar todas as extensões de VM de função disponíveis.
> 
> 

## <a name="find-add-update-and-remove-vm-extensions"></a>Localizar, adicionar, atualizar e remover extensões de VM
Para obter detalhes sobre essas tarefas, veja [Adicionar, encontrar, atualizar e remover Extensões de VM do Azure](../articles/virtual-machines/virtual-machines-windows-classic-manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).



<!--HONumber=Nov16_HO3-->


