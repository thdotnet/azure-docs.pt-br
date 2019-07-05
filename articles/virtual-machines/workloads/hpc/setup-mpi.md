---
title: Configurar o Message Passing Interface para HPC - máquinas virtuais do Azure | Microsoft Docs
description: Saiba como configurar o MPI para HPC no Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 5356a033dbc3d989dd27019f03b1fe36035ff9a4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441641"
---
# <a name="set-up-message-passing-interface-for-hpc"></a>Configurar o Message Passing Interface para HPC

Cargas de trabalho de mensagem (MPI Passing Interface) são uma parte significativa das cargas de trabalho HPC tradicionais. O SR-IOV habilitados tamanhos de VM no Azure permitem a praticamente qualquer tipo de MPI a ser usado. 

Executar trabalhos MPI em máquinas virtuais requer a configuração de chaves de partição (p-chaves) em um locatário. Siga as etapas a [descobrir as chaves de partição](#discover-partition-keys) seção para obter detalhes sobre como determinar os valores de chave p.

## <a name="ucx"></a>UCX

[UCX](https://github.com/openucx/ucx) oferece o melhor desempenho em IB e funciona com MPICH e OpenMPI.

```bash
wget https://github.com/openucx/ucx/releases/download/v1.4.0/ucx-1.4.0.tar.gz
tar -xvf ucx-1.4.0.tar.gz
cd ucx-1.4.0
./configure --prefix=<ucx-install-path>
make -j 8 && make install
```

## <a name="openmpi"></a>OpenMPI

Instale UCX conforme descrito anteriormente.

```bash
sudo yum install –y openmpi
```

OpenMPI de compilação.

```bash
wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.0.tar.gz
tar -xvf openmpi-4.0.0.tar.gz
cd openmpi-4.0.0
./configure --with-ucx=<ucx-install-path> --prefix=<ompi-install-path>
make -j 8 && make install
```

Execute OpenMPI.

```bash
<ompi-install-path>/bin/mpirun -np 2 --map-by node --hostfile ~/hostfile -mca pml ucx --mca btl ^vader,tcp,openib -x UCX_NET_DEVICES=mlx5_0:1  -x UCX_IB_PKEY=0x0003  ./osu_latency
```

Conforme mencionado acima, verifique sua chave de partição.

## <a name="mpich"></a>MPICH

Instale UCX conforme descrito anteriormente.

MPICH de compilação.

```bash
wget https://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz
tar -xvf mpich-3.3.tar.gz
cd mpich-3.3
./configure --with-ucx=<ucx-install-path> --prefix=<mpich-install-path> --with-device=ch4:ucx
make -j 8 && make install
```

MPICH em execução.

```bash
<mpich-install-path>/bin/mpiexec -n 2 -hostfile ~/hostfile -env UCX_IB_PKEY=0x0003 -bind-to hwthread ./osu_latency
```

Conforme mencionado acima, verifique sua chave de partição.

## <a name="mvapich2"></a>MVAPICH2

MVAPICH2 de compilação.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/mv2/mvapich2-2.3.tar.gz
tar -xv mvapich2-2.3.tar.gz
cd mvapich2-2.3
./configure --prefix=<mvapich2-install-path>
make -j 8 && make install
```

MVAPICH2 em execução.

```bash
<mvapich2-install-path>/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=48 ./osu_latency
```

## <a name="platform-mpi-community-edition"></a>Plataforma MPI Community Edition

Instale os pacotes necessários para o MPI da plataforma.

```bash
sudo yum install libstdc++.i686
sudo yum install glibc.i686
Download platform MPI at https://www.ibm.com/developerworks/downloads/im/mpi/index.html 
sudo ./platform_mpi-09.01.04.03r-ce.bin
```

Siga o processo de instalação.

## <a name="intel-mpi"></a>Intel MPI

[Baixar o Intel MPI](https://software.intel.com/mpi-library/choose-download).

Altere a variável de ambiente I_MPI_FABRICS dependendo da versão. Para o Intel MPI de 2018, use `I_MPI_FABRICS=shm:ofa` e para 2019, use `I_MPI_FABRICS=shm:ofi`.

Processo de fixação funciona corretamente para 15, 30 e 60 PPN por padrão.

## <a name="osu-mpi-benchmarks"></a>Parâmetros de comparação de MPI OSU

[Baixar Benchmarks OSU MPI] [ http://mvapich.cse.ohio-state.edu/benchmarks/ ](http://mvapich.cse.ohio-state.edu/benchmarks/) e descompactar.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/osu-micro-benchmarks-5.5.tar.gz
tar –xvf osu-micro-benchmarks-5.5.tar.gz
cd osu-micro-benchmarks-5.5
```

Parâmetros de comparação usando uma biblioteca específica de MPI da compilação:

```bash
CC=<mpi-install-path/bin/mpicc>CXX=<mpi-install-path/bin/mpicxx> ./configure 
make
```

Parâmetros de comparação de MPI estão sob `mpi/` pasta.


## <a name="discover-partition-keys"></a>Descubra as chaves de partição

Descubra as chaves de partição (p-chaves) para se comunicar com outras VMs.

```bash
/sys/class/infiniband/mlx5_0/ports/1/pkeys/0
/sys/class/infiniband/mlx5_0/ports/1/pkeys/1
```

O maior dos dois é a chave de locatário que deve ser usada com MPI. Exemplo: Se a seguir estão as chaves de p, 0x800b deve ser usado com MPI.

```bash
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/0
0x800b
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/1
0x7fff
```

Use a partição que não seja de chave de partição padrão (0x7fff). UCX requer o MSB do p-chave a ser apagado. Por exemplo, defina UCX_IB_PKEY como 0x000b para 0x800b.


## <a name="set-up-user-limits-for-mpi"></a>Configurar limites de usuário para o MPI

Configure limites de usuário para o MPI.

```bash
cat << EOF >> /etc/security/limits.conf
*               hard    memlock         unlimited
*               soft    memlock         unlimited
*               hard    nofile          65535
*               soft    nofile          65535
EOF
```


## <a name="set-up-ssh-keys-for-mpi"></a>Configurar chaves de SSH para o MPI

Configure chaves SSH para tipos MPI que precisam dele.

```bash
ssh-keygen -f /home/$USER/.ssh/id_rsa -t rsa -N ''
cat << EOF > /home/$USER/.ssh/config
Host *
    StrictHostKeyChecking no
EOF
cat /home/$USER/.ssh/id_rsa.pub >> /home/$USER/.ssh/authorized_keys
chmod 644 /home/$USER/.ssh/config
```

A sintaxe acima pressupõe uma pasta compartilhada de base, diretório de else. SSH deve ser copiado para cada nó.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) no Azure.
