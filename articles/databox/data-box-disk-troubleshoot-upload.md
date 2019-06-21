---
title: Problemas usando logs de carregar dados de solução de problemas de disco da caixa de dados do Azure | Microsoft Docs
description: Descreve como usar os logs e solucionar problemas encontrados ao carregar dados para o disco do Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 6f3ac38c3eac968bd2f7ec2aada435466d3ff279
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148303"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Compreender os logs para solucionar problemas de carregamento de dados no disco do Azure Data Box

Este artigo se aplica ao disco do Microsoft Azure Data Box e descreve os problemas que você vê quando você carrega dados no Azure.

## <a name="data-upload-logs"></a>Logs de carregamento de dados

Quando os dados são carregados para o Azure no datacenter, `_error.xml` e `_verbose.xml` arquivos são gerados. Esses logs são carregados para a mesma conta de armazenamento que foi usada para carregar dados. Um exemplo da `_error.xml` é mostrado abaixo.
    
```xml
 <?xml version="1.0" encoding="utf-8"?>
    <DriveLog Version="2018-10-01">
      <DriveId>18041C582D7E</DriveId>
      <Summary>
     <!--Summary for validation and data copy to Azure -->
        <ValidationErrors>
          <None Count="8" />
        </ValidationErrors>
        <CopyErrors>
          <Completed Count="5" Description="No errors encountered" />
          <ContainerRenamed Count="3" Description="Renamed the container as the original container name does not follow Azure conventions." />
        </CopyErrors>
      </Summary>
    <!--List of renamed containers with the new names, new file path in Azure, original invalid file path, and size -->
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/private.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\private.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/resource.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\resource.vhd</OriginalFilePath>
        <SizeInBytes>71528448</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/role.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\role.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Status>CompletedWithErrors</Status>
    </DriveLog>
```

## <a name="download-logs"></a>baixar logs

Há duas maneiras de localizar e baixar os logs de diagnóstico.

- Se houver erros ao carregar os dados para o Azure, o portal exibe um caminho para a pasta onde se encontram os logs de diagnóstico.

    ![Link logs no portal](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

- Vá para a conta de armazenamento associada com o seu pedido do Data Box. Vá até **Serviço Blob > Procurar blobs** e procure o blob correspondente à conta de armazenamento. Vá para **waies**.

    ![Copiar logs 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

Em cada caso, você pode ver os logs de erros e os logs detalhados. Selecione cada log e baixe uma cópia local.


## <a name="data-upload-errors"></a>Erros de upload de dados

Os erros gerados ao carregar os dados no Azure são resumidos na tabela a seguir.

| Código do erro | DESCRIÇÃO                        |
|-------------|------------------------------|
|`None` |  Foi concluída com êxito.           |
|`Renamed` | O blob foi renomeado com êxito.  |                                                            |
|`CompletedWithErrors` | Upload concluído com erros. Os detalhes dos arquivos de erro são incluídos no arquivo de log.  |
|`Corrupted`|CRC calculado durante a ingestão de dados não corresponde ao CRC calculado durante o carregamento.  |  
|`StorageRequestFailed` | Falha na solicitação de armazenamento do Azure.   |     |
|`LeasePresent` | Este item é concedido e está sendo usado por outro usuário. |
|`StorageRequestForbidden` |Não foi possível carregar devido a problemas de autenticação. |
|`ManagedDiskCreationTerminalFailure` | Não foi possível carregar como discos gerenciados. Os arquivos estão disponíveis na conta de armazenamento temporário, como blobs de página. Você pode converter manualmente blobs de página em discos gerenciados.  |
|`DiskConversionNotStartedTierInfoMissing` | Depois que o arquivo VHD foi copiado fora as pastas de camada de pré-criados, um disco gerenciado não tiver sido criado. O arquivo é carregado como um blob de páginas para a conta de armazenamento temporário, conforme especificado durante a criação do pedido. Você pode convertê-la manualmente em um disco gerenciado.|
|`InvalidWorkitem` | Não foi possível carregar os dados, pois ele não estiver de acordo com a nomenclatura do Azure e limita as convenções.|
|`InvalidPageBlobUploadAsBlockBlob` | Carregados como blobs de blocos em um contêiner com o prefixo `databoxdisk-invalid-pb-`.|
|`InvalidAzureFileUploadAsBlockBlob` | Carregados como blobs de blocos em um contêiner com o prefixo `databoxdisk-invalid-af`-.|
|`InvalidManagedDiskUploadAsBlockBlob` | Carregados como blobs de blocos em um contêiner com o prefixo `databoxdisk-invalid-md`-.|
|`InvalidManagedDiskUploadAsPageBlob` |Carregados como blobs de página em um contêiner com o prefixo `databoxdisk-invalid-md-`. |
|`MovedToOverflowShare` |Arquivos carregados para um novo compartilhamento de tamanho do compartilhamento original excederam o limite de tamanho máximo do Azure. O novo nome de compartilhamento de arquivo tem o nome do original com o sufixo `-2`.   |
|`MovedToDefaultAzureShare` |Arquivos carregados que não estavam uma parte de qualquer pasta em um compartilhamento de padrão. O nome do compartilhamento começa com `databox-`. |
|`ContainerRenamed` |O contêiner para esses arquivos não estão em conformidade com as convenções de nomenclatura do Azure e é renomeado. O novo nome começa com `databox-` e é sufixado com o hash SHA1 do nome original |
|`ShareRenamed` |O compartilhamento desses arquivos não estão em conformidade com as convenções de nomenclatura do Azure e é renomeado. O novo nome começa com `databox-` e é sufixado com o hash SHA1 do nome do original. |
|`BlobRenamed` |Esses arquivos não estão em conformidade com as convenções de nomenclatura do Azure e foram renomeados. Verifique o `BlobPath` campo para o novo nome. |
|`FileRenamed` |Esses arquivos não estão em conformidade com as convenções de nomenclatura do Azure e foram renomeados. Verifique o `FileStoragePath` campo para o novo nome. |
|`DiskRenamed` |Esses arquivos não estão em conformidade com as convenções de nomenclatura do Azure e foram renomeados. Verifique o `BlobPath` campo para o novo nome. |


## <a name="next-steps"></a>Próximas etapas

- [Abra um tíquete de suporte para problemas de disco Data Box](data-box-disk-contact-microsoft-support.md).
