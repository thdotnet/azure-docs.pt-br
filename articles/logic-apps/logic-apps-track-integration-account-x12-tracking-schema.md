---
title: Esquemas de acompanhamento X12 para mensagens B2B – Aplicativos Lógicos do Azure | Microsoft Docs
description: Criar esquemas de acompanhamento X12 que monitorem mensagens B2B nas contas de integração para os Aplicativos Lógicos do Azure com o Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.date: 01/27/2017
ms.openlocfilehash: 1db324006e1e6332b5fdd8afd28ebed8a32ac707
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60845759"
---
# <a name="create-schemas-for-tracking-x12-messages-in-integration-accounts-for-azure-logic-apps"></a>Criar esquemas para acompanhamento de mensagens X12 em contas de integração para os Aplicativos Lógicos do Azure

Você pode usar esses esquemas de acompanhamento X12 em sua conta de integração para ajudá-lo a monitorar êxito, erros e propriedades de mensagem para transações B2B (entre empresas):

* Esquema de acompanhamento do conjunto de transações X12
* Esquema de acompanhamento de confirmação do conjunto de transações X12
* Esquema de acompanhamento de intercâmbio X12
* Esquema de acompanhamento de confirmação do intercâmbio X12
* Esquema de acompanhamento de grupo funcional X12
* Esquema de acompanhamento de confirmação do grupo funcional X12

## <a name="x12-transaction-set-tracking-schema"></a>Esquema de acompanhamento do conjunto de transações X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "transactionSetControlNumber": "",
      "CorrelationMessageId": "",
      "messageType": "",
      "isMessageFailed": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "needAk2LoopForValidMessages":  "",
      "segmentsCount": ""
   }
}
```

| Propriedade | Type | DESCRIÇÃO |
| --- | --- | --- |
| senderPartnerName | String | O nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | String | O nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | String | Qualificador de parceiro de envio. (Obrigatório) |
| senderIdentifier | String | Identificador de parceiro de envio. (Obrigatório) |
| receiverQualifier | String | Qualificador de parceiro de recebimento. (Obrigatório) |
| receiverIdentifier | String | Identificador de parceiro de recebimento. (Obrigatório) |
| agreementName | String | Nome do contrato X12 para o qual as mensagens são resolvidas. (Opcional) |
| direction | Enum | Indica a direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | String | Número de controle de intercâmbio. (Opcional) |
| functionalGroupControlNumber | String | Número de controle funcional. (Opcional) |
| transactionSetControlNumber | String | Número de controle de conjunto de transações. (Opcional) |
| CorrelationMessageId | String | ID de mensagem de correlação. Uma combinação de {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber}. (Opcional) |
| messageType | String | Tipo de documento ou conjunto de transações. (Opcional) |
| isMessageFailed | Boolean | Se a mensagem X12 falha ou não. (Obrigatório) |
| isTechnicalAcknowledgmentExpected | Boolean | Se a confirmação técnica está configurada ou não no contrato X12. (Obrigatório) |
| isFunctionalAcknowledgmentExpected | Boolean | Se a confirmação funcional está configurada ou não no contrato X12. (Obrigatório) |
| needAk2LoopForValidMessages | Boolean | Se o loop AK2 é ou não necessário para uma mensagem válida. (Obrigatório) |
| segmentsCount | Número inteiro | O número de segmentos no conjunto de transações do X12. (Opcional) |
||||

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>Esquema de acompanhamento de confirmação do conjunto de transações X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "respondingtransactionSetControlNumber": "",
      "respondingTransactionSetId": "",
      "statusCode": "",
      "processingStatus": "",
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Propriedade | Type | DESCRIÇÃO |
| --- | --- | --- |
| senderPartnerName | String | O nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | String | O nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | String | Qualificador de parceiro de envio. (Obrigatório) |
| senderIdentifier | String | Identificador de parceiro de envio. (Obrigatório) |
| receiverQualifier | String | Qualificador de parceiro de recebimento. (Obrigatório) |
| receiverIdentifier | String | Identificador de parceiro de recebimento. (Obrigatório) |
| agreementName | String | Nome do contrato X12 para o qual as mensagens são resolvidas. (Opcional) |
| direction | Enum | Indica a direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | String | O número de controle de intercâmbio da confirmação funcional. O valor popula apenas o lado de envio em que a confirmação funcional é recebida para as mensagens enviadas ao parceiro. (Opcional) |
| functionalGroupControlNumber | String | O número de controle do grupo funcional da confirmação funcional. O valor popula apenas o lado de envio em que a confirmação funcional é recebida para as mensagens enviadas ao parceiro. (Opcional) |
| isaSegment | String | O segmento ISA da mensagem. O valor popula apenas o lado de envio em que a confirmação funcional é recebida para as mensagens enviadas ao parceiro. (Opcional) |
| gsSegment | String | O segmento GS da mensagem. O valor popula apenas o lado de envio em que a confirmação funcional é recebida para as mensagens enviadas ao parceiro. (Opcional) |
| respondingfunctionalGroupControlNumber | String | O número de controle de intercâmbio de resposta. (Opcional) |
| respondingFunctionalGroupId | String | A ID de grupo funcional de resposta, que mapeia para AK101 na confirmação. (Opcional) |
| respondingtransactionSetControlNumber | String | Número de controle de conjunto de transações de resposta. (Opcional) |
| respondingTransactionSetId | String | A ID de conjunto de transações de resposta, que mapeia para AK201 na confirmação. (Opcional) |
| statusCode | Boolean | O código de status de confirmação do conjunto de transações. (Obrigatório) |
| segmentsCount | Enum | O código de status de confirmação. Os valores aceitos são **Accepted**, **Rejected**, **AcceptedWithErrors**. (Obrigatório) |
| processingStatus | Enum | O status de processamento da confirmação. Os valores permitidos são **Received**, **Generated**, **Sent**. (Obrigatório) |
| CorrelationMessageId | String | ID de mensagem de correlação. Uma combinação de {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber}. (Opcional) |
| isMessageFailed | Boolean | Se a mensagem X12 falha ou não. (Obrigatório) |
| ak2Segment | String | A confirmação de um conjunto de transações no grupo funcional recebido. (Opcional) |
| ak3Segment | String | Relata erros em um segmento de dados. (Opcional) |
| ak5Segment | String | Relata se o conjunto de transações identificado no segmento AK2 foi aceito ou rejeitado e a razão. (Opcional) |
||||

## <a name="x12-interchange-tracking-schema"></a>Esquema de acompanhamento de intercâmbio X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "isa09": "",
      "isa10": "",
      "isa11": "",
      "isa12": "",
      "isa14": "",
      "isa15": "",
      "isa16": ""
   }
}
```

| Propriedade | Type | DESCRIÇÃO |
| --- | --- | --- |
| senderPartnerName | String | O nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | String | O nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | String | Qualificador de parceiro de envio. (Obrigatório) |
| senderIdentifier | String | Identificador de parceiro de envio. (Obrigatório) |
| receiverQualifier | String | Qualificador de parceiro de recebimento. (Obrigatório) |
| receiverIdentifier | String | Identificador de parceiro de recebimento. (Obrigatório) |
| agreementName | String | Nome do contrato X12 para o qual as mensagens são resolvidas. (Opcional) |
| direction | Enum | Indica a direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | String | Número de controle de intercâmbio. (Opcional) |
| isaSegment | String | Segmento ISA de mensagem. (Opcional) |
| isTechnicalAcknowledgmentExpected | Boolean | Se a confirmação técnica está configurada ou não no contrato X12. (Obrigatório) |
| isMessageFailed | Boolean | Se a mensagem X12 falha ou não. (Obrigatório) |
| isa09 | String | A data de intercâmbio do documento X12. (Opcional) |
| isa10 | String | A hora de intercâmbio do documento X12. (Opcional) |
| isa11 | String | O identificador de Padrões de Controle de intercâmbio X12. (Opcional) |
| isa12 | String | O número de versão de controle de intercâmbio X12. (Opcional) |
| isa14 | String | A confirmação do X12 é solicitada. (Opcional) |
| isa15 | String | O indicador de teste ou produção. (Opcional) |
| isa16 | String | Separador de elementos. (Opcional) |
||||

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>Esquema de acompanhamento de confirmação do intercâmbio X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "respondingInterchangeControlNumber": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ta102": "",
      "ta103": "",
      "ta105": ""
   }
}
```

| Propriedade | Type | DESCRIÇÃO |
| --- | --- | --- |
| senderPartnerName | String | O nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | String | O nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | String | Qualificador de parceiro de envio. (Obrigatório) |
| senderIdentifier | String | Identificador de parceiro de envio. (Obrigatório) |
| receiverQualifier | String | Qualificador de parceiro de recebimento. (Obrigatório) |
| receiverIdentifier | String | Identificador de parceiro de recebimento. (Obrigatório) |
| agreementName | String | Nome do contrato X12 para o qual as mensagens são resolvidas. (Opcional) |
| direction | Enum | Indica a direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | String | O número de controle de intercâmbio da confirmação técnica recebida dos parceiros. (Opcional) |
| isaSegment | String | O segmento ISA da confirmação técnica recebida dos parceiros. (Opcional) |
| respondingInterchangeControlNumber |String | O número de controle de intercâmbio da confirmação técnica recebida dos parceiros. (Opcional) |
| isMessageFailed | Boolean | Se a mensagem X12 falha ou não. (Obrigatório) |
| statusCode | Enum | O código de status de confirmação do intercâmbio. Os valores aceitos são **Accepted**, **Rejected**, **AcceptedWithErrors**. (Obrigatório) |
| processingStatus | Enum | Status de confirmação. Os valores permitidos são **Received**, **Generated**, **Sent**. (Obrigatório) |
| ta102 | String | Data do intercâmbio. (Opcional) |
| ta103 | String | Hora do intercâmbio. (Opcional) |
| ta105 | String | Código de observação de intercâmbio. (Opcional) |
||||

## <a name="x12-functional-group-tracking-schema"></a>Esquema de acompanhamento de grupo funcional X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "gsSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "gs01": "",
      "gs02": "",
      "gs03": "",
      "gs04": "",
      "gs05": "",
      "gs07": "",
      "gs08": ""
   }
}
```

| Propriedade | Type | DESCRIÇÃO |
| --- | --- | --- |
| senderPartnerName | String | O nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | String | O nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | String | Qualificador de parceiro de envio. (Obrigatório) |
| senderIdentifier | String | Identificador de parceiro de envio. (Obrigatório) |
| receiverQualifier | String | Qualificador de parceiro de recebimento. (Obrigatório) |
| receiverIdentifier | String | Identificador de parceiro de recebimento. (Obrigatório) |
| agreementName | String | Nome do contrato X12 para o qual as mensagens são resolvidas. (Opcional) |
| direction | Enum | Indica a direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | String | Número de controle de intercâmbio. (Opcional) |
| functionalGroupControlNumber | String | Número de controle funcional. (Opcional) |
| gsSegment | String | Segmento GS de mensagem. (Opcional) |
| isTechnicalAcknowledgmentExpected | Boolean | Se a confirmação técnica está configurada ou não no contrato X12. (Obrigatório) |
| isFunctionalAcknowledgmentExpected | Boolean | Se a confirmação funcional está configurada ou não no contrato X12. (Obrigatório) |
| isMessageFailed | Boolean | Se a mensagem X12 falha ou não. (Obrigatório)|
| gs01 | String | O código do identificador funcional. (Opcional) |
| gs02 | String | O código do remetente do aplicativo. (Opcional) |
| gs03 | String | O código do receptor do aplicativo. (Opcional) |
| gs04 | String | Data de grupo funcional. (Opcional) |
| gs05 | String | Hora de grupo funcional. (Opcional) |
| gs07 | String | Código da agência responsável. (Opcional) |
| gs08 | String | Código de identificador de versão/lançamento/setor. (Opcional) |
||||

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>Esquema de acompanhamento de confirmação do grupo funcional X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ak903": "",
      "ak904": "",
      "ak9Segment": ""
   }
}
```

| Propriedade | Type | DESCRIÇÃO |
| --- | --- | --- |
| senderPartnerName | String | O nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | String | O nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | String | Qualificador de parceiro de envio. (Obrigatório) |
| senderIdentifier | String | Identificador de parceiro de envio. (Obrigatório) |
| receiverQualifier | String | Qualificador de parceiro de recebimento. (Obrigatório) |
| receiverIdentifier | String | Identificador de parceiro de recebimento. (Obrigatório) |
| agreementName | String | Nome do contrato X12 para o qual as mensagens são resolvidas. (Opcional) |
| direction | Enum | Indica a direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | String | Número de controle de intercâmbio, que popula o lado de envio quando uma confirmação técnica é recebida de parceiros. (Opcional) |
| functionalGroupControlNumber | String | Número de controle de grupo funcional da confirmação técnica, que popula o lado de envio quando uma confirmação técnica é recebida de parceiros. (Opcional) |
| isaSegment | String | O mesmo que o número de controle de intercâmbio, só é populado em casos específicos. (Opcional) |
| gsSegment | String | O mesmo que o número de controle de grupo funcional, só é populado em casos específicos. (Opcional) |
| respondingfunctionalGroupControlNumber | String | Número de controle do grupo funcional original. (Opcional) |
| respondingFunctionalGroupId | String | Mapeia para AK101 na ID do grupo funcional de confirmação. (Opcional) |
| isMessageFailed | Boolean | Se a mensagem X12 falha ou não. (Obrigatório) |
| statusCode | Enum | O código de status de confirmação. Os valores aceitos são **Accepted**, **Rejected**, **AcceptedWithErrors**. (Obrigatório) |
| processingStatus | Enum | O status de processamento da confirmação. Os valores permitidos são **Received**, **Generated**, **Sent**. (Obrigatório) |
| ak903 | String | Número de conjuntos de transação recebidos. (Opcional) |
| ak904 | String | Indica o número de conjuntos de transação aceitos no grupo funcional identificado. (Opcional) |
| ak9Segment | String | Se o grupo funcional identificado no segmento AK1 foi aceito ou rejeitado e a razão. (Opcional) |
|||| 

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de acompanhamento do protocolo B2B

Para obter informações sobre esquemas de acompanhamento do protocolo B2B, veja:

* [Esquemas de acompanhamento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [Esquemas de acompanhamento B2B personalizados](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre o [monitoramento de mensagens de B2B](logic-apps-monitor-b2b-message.md).
* Saiba mais sobre [rastreando mensagens B2B nos logs do Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
