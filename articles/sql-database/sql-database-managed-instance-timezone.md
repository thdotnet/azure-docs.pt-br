---
title: Instância Gerenciada do Banco de Dados SQL do Azure fusos horários | Microsoft Docs "
description: Saiba mais sobre as especificações de fuso horário do Instância Gerenciada do Banco de Dados SQL do Azure
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
ms.date: 08/14/2019
ms.openlocfilehash: a02709ffde144e7bd5e4d05fcd0e07c5d84a15fb
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035824"
---
# <a name="time-zones-in-azure-sql-database-managed-instance"></a>Fusos horários em Instância Gerenciada do Banco de Dados SQL do Azure

UTC (tempo Universal Coordenado) é o fuso horário recomendado para a camada de dados das soluções de nuvem. O Instância Gerenciada do Banco de Dados SQL do Azure também oferece uma escolha de fusos horários para atender às necessidades de aplicativos existentes que armazenam valores de data e hora e funções de data e hora com um contexto implícito de um fuso horário específico.

Funções T-SQL como [GETDATE ()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) ou código CLR observam o fuso horário definido no nível da instância. SQL Server Agent trabalhos também seguem agendas de acordo com o fuso horário da instância.

  >[!NOTE]
  > Instância Gerenciada é a única opção de implantação do banco de dados SQL do Azure que dá suporte à configuração de fuso horário. Outras opções de implantação sempre seguem o UTC.
Use [no fuso horário](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) em bancos de dados SQL únicos e em pool se você precisar interpretar informações de data e hora em um fuso horário não UTC.

## <a name="supported-time-zones"></a>Fusos horários com suporte

Um conjunto de fusos horários com suporte é herdado do sistema operacional subjacente da instância gerenciada. Ele é atualizado regularmente para obter novas definições de fuso horário e refletir as alterações nas existentes.

A [política de horário de verão e alterações de fuso horário](https://aka.ms/time) garante a precisão histórica de 2010 para frente.

Uma lista com nomes dos fusos horários com suporte é exposta por meio da exibição do sistema [Sys. time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) .

## <a name="set-a-time-zone"></a>Definir um fuso horário

Um fuso horário de uma instância gerenciada pode ser definido somente durante a criação da instância. O fuso horário padrão é UTC.

  >[!NOTE]
  > O fuso horário de uma instância gerenciada existente não pode ser alterado.

### <a name="set-the-time-zone-through-the-azure-portal"></a>Definir o fuso horário por meio do portal do Azure

Ao inserir parâmetros para uma nova instância, selecione um fuso horário na lista de fusos horários com suporte.
  
![Definindo um fuso horário durante a criação da instância](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Modelo do Azure Resource Manager

Especifique a propriedade TimeZoneID em seu [modelo do Resource Manager](https://aka.ms/sql-mi-create-arm-posh) para definir o fuso horário durante a criação da instância.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Uma lista de valores com suporte para a propriedade TimeZoneID está no final deste artigo.

Se não for especificado, o fuso horário será definido como UTC.

## <a name="check-the-time-zone-of-an-instance"></a>Verificar o fuso horário de uma instância

A função [CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql) retorna um nome de exibição do fuso horário da instância.

## <a name="cross-feature-considerations"></a>Considerações sobre recursos cruzados

### <a name="restore-and-import"></a>Restaurar e importar

Você pode restaurar um arquivo de backup ou importar dados para uma instância gerenciada de uma instância do ou um servidor com configurações de fuso horário diferentes. Certifique-se de fazer isso com cautela. Analise o comportamento do aplicativo e os resultados das consultas e dos relatórios, assim como quando você transfere dados entre duas instâncias de SQL Server com configurações de fuso horário diferentes.

### <a name="point-in-time-restore"></a>Restauração pontual

<del>Quando você executa uma restauração pontual, o tempo para restaurar é interpretado como hora UTC. Dessa forma, quaisquer ambiguidades devido ao horário de verão e suas possíveis alterações são evitadas.<del>

 >[!WARNING]
  > O comportamento atual não está em linha com a instrução acima, e o tempo para restaurar é interpretado de acordo com o fuso horário da instância gerenciada de origem na qual os backups automáticos de banco de dados são obtidos. Estamos trabalhando para corrigir esse comportamento para interpretar determinado ponto no tempo como hora UTC.

### <a name="auto-failover-groups"></a>Grupos de failover automático

Usar o mesmo fuso horário em uma instância primária e secundária em um grupo de failover não é imposto, mas é altamente recomendável.

  >[!WARNING]
  > É altamente recomendável que você use o mesmo fuso horário para a instância primária e secundária em um grupo de failover. Devido a alguns cenários raros, manter o mesmo fuso horário em instâncias primárias e secundárias não é imposto. É importante entender que, no caso de failover manual ou automático, a instância secundária manterá seu fuso horário original.

## <a name="limitations"></a>Limitações

- O fuso horário da instância gerenciada existente não pode ser alterado.
- Os processos externos iniciados por meio dos trabalhos de SQL Server Agent não observam o fuso horário da instância.

## <a name="known-issues"></a>Problemas conhecidos

Quando a operação de restauração pontual (PITR) é executada, o tempo de restauração é interpretado como por conjunto de zonas de tempo na instância gerenciada na qual os backups automáticos de banco de dados são obtidos, embora a página do portal para PITR sugira que o tempo seja interpretado como UTC.

Exemplo:

Digamos que a instância em que os backups automáticos são obtidos tem fuso horário padrão do leste (UTC-5).
A página do portal para a restauração pontual sugere que a hora em que você está optando por restaurar é hora UTC:

![PITR com a hora local usando o portal](media/sql-database-managed-instance-timezone/02-pitr-with-nonutc-timezone.png)

No entanto, o tempo para restaurar é realmente interpretado como hora padrão do leste e, neste banco de dados de exemplo específico, será restaurado para o estado às 9h, horário padrão do leste e não hora UTC.

Se você quiser fazer uma restauração pontual em um ponto específico em hora UTC, primeiro Calcule o tempo equivalente no fuso horário da instância de origem e use esse tempo no portal ou no script PowerShell/CLI.

## <a name="list-of-supported-time-zones"></a>Lista de fusos horários com suporte

| **ID do fuso horário** | **Nome de exibição do fuso horário** |
| --- | --- |
| Hora Oficial do Meridiano de Data | (UTC-12:00) Linha de Data Internacional do Oeste |
| UTC-11 | (UTC-11:00) Tempo Universal Coordenado -11 |
| Horário Padrão das Aleutas | (UTC-10:00) Ilhas Aleutas |
| Hora Oficial do Havaí | (UTC-10:00) Havaí |
| Horário Padrão de Marquesas | (UTC-09:30) Ilhas Marquesas |
| Hora Oficial do Alasca | (UTC-09:00) Alasca |
| UTC-09 | (UTC-09:00) Tempo Universal Coordenado-09 |
| Horário Padrão do Pacífico (México) | (UTC-08:00) Baja California |
| UTC-08 | (UTC-08:00) Tempo Universal Coordenado-08 |
| Hora Oficial do Pacífico | (UTC-08:00) Hora do Pacífico (EUA e Canadá) |
| Hora Padrão das Montanhas dos EUA | (UTC-07:00) Arizona |
| Hora Padrão das Montanhas (México) | (UTC-07:00) Chihuahua, La Paz, Mazatlán |
| Hora Oficial das Montanhas | (UTC-07:00) Hora das Montanhas (EUA e Canadá) |
| Hora Oficial da América Central | (UTC-06:00) América Central |
| Hora Oficial Central | (UTC-06:00) Hora Central (EUA e Canadá) |
| Horário Padrão da Ilha da Páscoa | (UTC-06:00) Ilha de Páscoa |
| Hora Padrão Central (México) | (UTC-06:00) Guadalajara, Cidade do México, Monterrey |
| Hora Oficial Canadá Central | (UTC-06:00) Saskatchewan |
| Hora Padrão do Pacífico na América do Sul | (UTC-05:00) Bogotá, Lima, Quito, Rio Branco |
| Hora Padrão Oriental (México) | (UTC-05:00) Chetumal |
| Hora Oficial do Leste dos EUA | (UTC-05:00) Hora do Leste (EUA e Canadá) |
| Horário Padrão do Haiti | (UTC-05:00) Haiti |
| Horário Padrão de Cuba | (UTC-05:00) Havana |
| Hora Padrão do Leste dos EUA | (UTC-05:00) Indiana (Leste) |
| Hora oficial turcos e Caicos | (UTC-05:00) Ilhas Turks e Caicos |
| Horário Padrão do Paraguai | (UTC-04:00) Assunção |
| Hora Oficial do Atlântico | (UTC-04:00) Hora do Atlântico (Canadá) |
| Horário Padrão da Venezuela | (UTC-04:00) Caracas |
| Horário Padrão do Brasil Central | (UTC-04:00) Cuiabá |
| Hora Padrão do Oeste da América do Sul | (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Hora Padrão do Pacífico SA | (UTC-04:00) Santiago |
| Hora Padrão de Newfoundland | (UTC-03:30) Newfoundland |
| Horário Padrão de Tocantins | (UTC-03:00) Araguaína |
| E. Hora oficial da América do Sul | (UTC-03:00) Brasília |
| Hora Padrão do Leste da América do Sul | (UTC-03:00) Caiena, Fortaleza |
| Horário Padrão da Argentina | (UTC-03:00) Cidade de Buenos Aires |
| Hora Oficial da Groelândia | (UTC-03:00) Groenlândia |
| Horário Padrão de Montevidéu | (UTC-03:00) Montevidéu |
| Hora Padrão de Magallanes | (UTC-03:00) Punta Arenas |
| Horário Padrão de Saint Pierre | (UTC-03:00) São Pedro e Miquelon |
| Hora Padrão da Bahia | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Tempo Universal Coordenado -02 |
| Hora Oficial do Atlântico Central | (UTC-02:00) Atlântico Central - Antiga |
| Hora Oficial dos Açores | (UTC-01:00) Açores |
| Hora oficial do cabo verde | (UTC-01:00) Ilhas de Cabo Verde |
| UTC | (UTC) Tempo Universal Coordenado |
| Hora Oficial GMT | (UTC+00:00) Dublin, Edimburgo, Lisboa, Londres |
| Hora de Greenwich | (UTC+00:00) Monróvia, Reykjavik |
| W. Hora padrão da Europa | (UTC+01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Hora Oficial da Europa Central | (UTC+01:00) Belgrado, Bratislava, Budapeste, Ljubljana, Praga |
| Hora Oficial Românica | (UTC+01:00) Bruxelas, Copenhague, Madri, Paris |
| Horário Padrão do Marrocos | (UTC+01:00) Casablanca |
| Hora Oficial de São Tomé | (UTC+01:00) São Tomé |
| Hora Oficial da Europa Central | (UTC+01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| W. Hora oficial da África Central | (UTC+01:00) Centro-oeste da África |
| Horário Padrão da Jordânia | (UTC+02:00) Amã |
| Hora Oficial GTB | (UTC+02:00) Atenas, Bucareste |
| Horário Padrão do Oriente Médio | (UTC+02:00) Beirute |
| Hora Oficial do Egito | (UTC+02:00) Cairo |
| E. Hora padrão da Europa | (UTC+02:00) Chisinau |
| Hora Padrão da Síria | (UTC+02:00) Damasco |
| Hora oficial do banco oeste | (UTC+02:00) Gaza, Hebron |
| Hora Oficial da África do Sul | (UTC+02:00) Harare, Pretória |
| Hora Oficial FLE | (UTC+02:00) Helsinque, Kiev, Riga, Sofia, Tallinn, Vilnius |
| Hora padrão de Israel | (UTC+02:00) Jerusalém |
| Hora padrão de Caliningrado | (UTC+02:00) Kaliningrado |
| Hora Padrão do Sudão | (UTC+02:00) Cartum |
| Hora Padrão da Líbia | (UTC+02:00) Trípoli |
| Horário Padrão da Namíbia | (UTC+02:00) Windhoek |
| Hora Oficial Arábica | (UTC+03:00) Bagdá |
| Hora Padrão da Turquia | (UTC+03:00) Istambul |
| Hora Oficial Árabe | (UTC+03:00) Kuwait, Riad |
| Hora Padrão de Belarus | (UTC+03:00) Minsk |
| Hora oficial russa | (UTC+03:00) Moscou, São Petersburgo |
| E. Hora padrão da África | (UTC+03:00) Nairóbi |
| Hora Oficial do Irã | (UTC+03:30) Teerã |
| Hora Oficial Árabe | (UTC+04:00) Abu Dhabi, Mascate |
| Horário Padrão de Astrakhan | (UTC+04:00) Astrakhan, Ulyanovsk |
| Horário Padrão do Azerbaijão | (UTC+04:00) Baku |
| Zona 3 de tempo da Rússia | (UTC+04:00) Izhevsk, Samara |
| Horário Padrão de Maurício | (UTC+04:00) Port Louis |
| Hora Padrão de Saratov | (UTC+04:00) Saratov |
| Horário Padrão da Geórgia | (UTC+04:00) Tbilisi |
| Hora Oficial de Volgogrado | (UTC+04:00) Volgogrado |
| Hora Oficial do Cáucaso | (UTC+04:00) Ierevan |
| Hora Padrão do Afeganistão | (UTC+04:30) Cabul |
| Hora Oficial do Oeste da Ásia | (UTC+05:00) Ashgabat, Tashkent |
| Hora padrão de Ecaterimburgo | (UTC+05:00) Ekaterinburg |
| Horário Padrão do Paquistão | (UTC+05:00) Islamabad, Karachi |
| Hora Oficial da Índia | (UTC+05:30) Chennai, Calcutá, Mumbai, Nova Délhi |
| Hora Oficial do Sri Lanka | (UTC+05:30) Sri Jayawardenepura |
| Hora Oficial do Nepal | (UTC+05:45) Katmandu |
| Hora Oficial da Ásia Central | (UTC+06:00) Astana |
| Hora Padrão de Bangladesh | (UTC+06:00) Dacca |
| Horário Padrão de Omsk | (UTC+06:00) Omsk |
| Hora Oficial de Myanmar | (UTC+06:30) Yangon (Rangoon) |
| Hora Padrão do Sudeste Asiático | (UTC+07:00) Bancoc, Hanói, Jakarta |
| Horário Padrão de Altai | (UTC+07:00) Barnaul, Gorno-Altaysk |
| W. Hora padrão da Mongólia | (UTC+07:00) Hovd |
| Hora oficial do norte da Ásia | (UTC+07:00) Krasnoyarsk |
| P. Hora Oficial da Ásia Central | (UTC+07:00) Novosibirsk |
| Horário Padrão de Tomsk | (UTC+07:00) Tomsk |
| Hora Oficial da China | (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Hora padrão do norte da Ásia Oriental | (UTC+08:00) Irkutsk |
| Hora oficial de Cingapura | (UTC+08:00) Kuala Lumpur, Cingapura |
| W. Hora padrão da Austrália | (UTC+08:00) Perth |
| Hora Oficial de Taipei | (UTC+08:00) Taipé |
| Hora Padrão de Ulan Bator | (UTC+08:00) Ulan Bator |
| Horário Padrão da Austrália Ocidental | (UTC+08:45) Eucla |
| Horário Padrão de Transbaikal | (UTC+09:00) Chita |
| Hora Oficial de Tóquio | (UTC+09:00) Osaka, Sapporo, Tóquio |
| Hora Padrão da Coreia do Norte | (UTC + 09:00) Pyongyang |
| Hora Oficial da Coreia | (UTC+09:00) Seul |
| Hora padrão de Yakutsk | (UTC+09:00) Yakutsk |
| Central. Hora padrão da Austrália | (UTC+09:30) Adelaide |
| Hora Padrão da Austrália Central | (UTC+09:30) Darwin |
| E. Hora padrão da Austrália | (UTC+10:00) Brisbane |
| Hora Padrão da Austrália Oriental | (UTC+10:00) Canberra, Melbourne, Sydney |
| Hora Oficial do Oeste do Pacífico | (UTC+10:00) Guam, Port Moresby |
| Hora Oficial da Tasmânia | (UTC+10:00) Hobart |
| Hora padrão de Vladivostok | (UTC+10:00) Vladivostok |
| Horário Padrão de Lord Howe | (UTC+10:30) Ilha Lord Howe |
| Horário Padrão de Bougainville | (UTC+11:00) Ilha Bougainville |
| Fuso horário de Rússia 10 | (UTC+11:00) Chokurdakh |
| Hora Padrão de Magadã | (UTC+11:00) Magadan |
| Horário Padrão de Norfolk | (UTC+11:00) Ilha Norfolk |
| Horário Padrão de Sakhalin | (UTC+11:00) Sakhalin |
| Hora Oficial do Pacífico Central | (UTC+11:00) Ilhas Salomão, Nova Caledônia |
| Fuso horário da Rússia 11 | (UTC+12:00) Anadyr, Petropavlovsk-Kamchatsky |
| Hora Oficial da Nova Zelândia | (UTC+12:00) Auckland, Wellington |
| UTC+12 | (UTC+12:00) Tempo Universal Coordenado +12 |
| Hora Padrão de Fiji | (UTC+12:00) Fiji |
| Horário Padrão de Kamchatka | (UTC+12:00) Petropavlovsk-Kamchatsky - Antiga |
| Horário Padrão das Ilhas Chatham | (UTC+12:45) Ilhas Chatham |
| UTC+13 | (UTC+13:00) Tempo Universal Coordenado+13 |
| Hora Oficial de Tonga | (UTC+13:00) Nuku'alofa |
| Hora Oficial de Samoa | (UTC+13:00) Samoa |
| Hora Padrão das Ilhas Line | (UTC+14:00) Ilha de Kiritimati |

## <a name="see-also"></a>Consulte também 

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [AT TIME ZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)
