---
description: Codes d’erreur de l’objet DataControl
title: Codes d’erreur DataControl | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], DataControl
- DataControl errors [ADO]
ms.assetid: 293df9d5-e1a2-406d-9107-07bf7cdc6f96
author: rothja
ms.author: jroth
ms.openlocfilehash: dbcf0b6aef651e90c90df1cb49af2dbe6a29c674
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029433"
---
# <a name="datacontrol-object-error-codes"></a>Codes d’erreur de l’objet DataControl
Le tableau suivant répertorie les [services Bureau à distance. ](../../reference/rds-api/datacontrol-object-rds.md) Codes d’erreur de l’objet DataControl. La traduction décimale positive des deux octets de poids faible, la traduction décimale négative du code d’erreur complet et les valeurs hexadécimales sont affichées.

|Licence. Codes d’erreur DataControl|Number|Description|
|---------------------------------|------------|-----------------|
|**IDS_AsyncPending**|4107 -2146824175 0x800A1011|Impossible d’effectuer l’opération alors que l’opération asynchrone est en attente.|
|**IDS_BadInlineTablegram**|4105 -2146824183 0x800A1009|Inline TableGram incorrect.|
|**IDS_CantConnect**|4099 -2146824189 0x800A1003|Impossible de se connecter au serveur.|
|**IDS_CantCreateObject**|4100-2146824188 0x800A1004|Impossible de créer l’objet métier.|
|**IDS_CantFindDataspace**|4102 -2146824186 0x800A1006|La propriété DataSpace n’est pas valide.|
|**IDS_CantInvokeMethod**|4101 -2146824187 0x800A1005|La méthode ne peut pas être appelée sur un objet métier.|
|**IDS_CrossDomainWarning**|4112 -2146824170 0x800A1016|Cette page accède aux données d’un autre domaine. Voulez-vous autoriser cette opération ? Pour éviter ce message dans Internet Explorer, vous pouvez ajouter un site Web sécurisé à votre zone de sites de confiance sous l’onglet **sécurité** de la boîte de dialogue **Options Internet** .|
|**IDS_InvalidADCClientVersion**|4106 -2146824176 0x800A1010|Version du client RDS non valide : le client est plus récent que le serveur.|
|**IDS_INVALIDARG**|5376 -2147019520 0x80071500|Un ou plusieurs arguments ne sont pas valides.|
|**IDS_InvalidBindings**|4097-2146824191 0x800A1001|Erreur dans la propriété Bindings.|
|**IDS_InvalidParam**|4110 -2146824172 0x800A1014|Un ou plusieurs arguments ne sont pas valides.|
|**IDS_NOINTERFACE**|5377-2147019519 0x80071501|Aucune interface de ce type n’est prise en charge.|
|**IDS_NotReentrant**|4111-2146824171 0x800A1015|La requête ne peut pas être exécutée tant que le gestionnaire d’événements est toujours en cours de traitement.|
|**IDS_ObjectNotSafe**|4103-2146824185 0x800A1007|Les paramètres de sécurité de cet ordinateur interdisent la création de l’objet métier.|
|**IDS_RecordsetNotOpen**|4109-2146824173 0x800A1013|Le **jeu d’enregistrements** n’est pas ouvert.|
|**IDS_ResetInvalidField**|4108 -2146824174 0x800A1012|La colonne spécifiée dans **SortColumn** ou **FilterColumn** n’existe pas.|
|**IDS_RowsetNotUpdateable**|4104 -2146824184 0x800A1008|L’ensemble de lignes n’est pas modifiable.|
|**IDS_UnexpectedError**|4351-2146823937 0x800A10FF|Erreur inattendue.|
|**IDS_UpdatesFailed**|4098 -2146824190 0x800A1002|Impossible de mettre à jour la base de données.|
|**IDS_URLMONNotFound**|4119 -2146824169 0x800A1017|La propriété de l' **URL** DataControl requiert le fichier système Urlmon.dll, qui est introuvable.|

## <a name="see-also"></a>Voir aussi
 [DataControl, objet (RDS)](../../reference/rds-api/datacontrol-object-rds.md)