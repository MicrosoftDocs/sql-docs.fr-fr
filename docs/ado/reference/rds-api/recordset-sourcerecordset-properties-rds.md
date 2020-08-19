---
description: Recordset et SourceRecordset, propriétés (RDS)
title: Recordset, SourceRecordset, propriétés (RDS) | Microsoft Docs
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Recordset property [ADO]
ms.assetid: a29e3fb9-306d-497a-9a59-1856a914e5e9
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d64488fa60b6cc6bcdd7c45124a7dcedb60d4c2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88438731"
---
# <a name="recordset-sourcerecordset-properties-rds"></a>Recordset et SourceRecordset, propriétés (RDS)
Indique l’objet **Recordset** renvoyé à partir d’un objet métier personnalisé.  
  
 **S’applique à :** [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.SourceRecordset = Recordset  
Recordset = DataControl.Recordset   
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) .  
  
 *Recordset*  
 Variable objet qui représente un objet **Recordset** .  
  
## <a name="remarks"></a>Notes  
 Vous pouvez définir la propriété **SourceRecordset** sur un [jeu d’enregistrements](../../../ado/reference/ado-api/recordset-object-ado.md) retourné à partir d’un objet métier personnalisé.  
  
 Ces propriétés permettent à une application de gérer le processus de liaison au moyen d’un processus personnalisé. Ils reçoivent un ensemble de lignes encapsulé dans un **Recordset** afin que vous puissiez interagir directement avec le **Recordset**, en effectuant des actions telles que la définition de propriétés ou l’itération dans le **jeu d’enregistrements**.  
  
 Vous pouvez définir la propriété **SourceRecordset** ou lire la propriété **Recordset** au moment de l’exécution dans le code de script.  
  
 **SourceRecordset** est une propriété en écriture seule, contrairement à la propriété **Recordset** , qui est une propriété en lecture seule.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Recordset et SourceRecordset, exemples de propriétés (VBScript)](../../../ado/reference/rds-api/recordset-and-sourcerecordset-properties-example-vbscript.md)   
 [CreateRecordset, méthode (RDS)](../../../ado/reference/rds-api/createrecordset-method-rds.md)   
 [Query, méthode (RDS)](../../../ado/reference/rds-api/query-method-rds.md)


