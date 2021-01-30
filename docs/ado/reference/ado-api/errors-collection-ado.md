---
description: Errors, collection (ADO)
title: Errors, collection (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Errors
- Connection15::get_Errors
- Connection15::GetErrors
helpviewer_keywords:
- Errors collection [ADO]
ms.assetid: 290819e1-7b39-4e1e-a93b-801257138b00
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f75d1a068a3e86ecb9e0d5b1e27afae6ac4a6e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171190"
---
# <a name="errors-collection-ado"></a>Errors, collection (ADO)
Contient tous les objets d' [erreur](../../../ado/reference/ado-api/error-object.md) créés en réponse à un échec lié à un fournisseur unique.  
  
## <a name="remarks"></a>Notes  
 Toute opération impliquant des objets ADO peut générer une ou plusieurs erreurs de fournisseur. À mesure que chaque erreur se produit, un ou plusieurs objets **Error** peuvent être placés dans la collection **Errors** de l’objet [Connection](../../../ado/reference/ado-api/connection-object-ado.md) . Lorsqu’une autre opération ADO génère une erreur, la collection **Errors** est désactivée et le nouvel ensemble d’objets **Error** peut être placé dans la collection **Errors** .  
  
 Chaque objet **Error** représente une erreur de fournisseur spécifique, et non une erreur ADO. Les erreurs ADO sont exposées au mécanisme de gestion des exceptions au moment de l’exécution. Par exemple, dans Microsoft Visual Basic, l’occurrence d’une erreur spécifique à ADO déclenchera un événement [OnError](../../../ado/reference/rds-api/onerror-event-rds.md) et apparaîtra dans l’objet **Err** .  
  
 Les opérations ADO qui ne génèrent pas d’erreur n’ont aucun effet sur la collection **Errors** . Utilisez la méthode [Clear](../../../ado/reference/ado-api/clear-method-ado.md) pour effacer manuellement la collection **Errors** .  
  
 L’ensemble d’objets **Error** de la collection **Errors** décrit toutes les erreurs qui se sont produites en réponse à une instruction unique. L’énumération des erreurs spécifiques dans la collection d' **Erreurs** permet à vos routines de gestion des erreurs de déterminer plus précisément la cause et l’origine d’une erreur et de prendre les mesures appropriées pour effectuer la récupération.  
  
 Certaines propriétés et méthodes retournent des avertissements qui s’affichent en tant qu’objets d' **erreur** dans la collection d' **Erreurs** , mais n’arrêtent pas l’exécution d’un programme. Avant d’appeler les méthodes [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)ou [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) sur un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) , la méthode [Open](../../../ado/reference/ado-api/open-method-ado-connection.md) sur un objet **Connection** ou définir la propriété [Filter](../../../ado/reference/ado-api/filter-property.md) sur un objet **Recordset** , appelez la méthode **Clear** sur la collection **Errors** . De cette façon, vous pouvez lire la propriété [Count](../../../ado/reference/ado-api/count-property-ado.md) de la collection **Errors** pour tester les avertissements retournés.  
  
> [!NOTE]
>  Pour une explication plus détaillée de la façon dont une seule opération ADO peut générer plusieurs erreurs, consultez la rubrique relative à l’objet d' **erreur** .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Errors](../../../ado/reference/ado-api/errors-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Objet Error](../../../ado/reference/ado-api/error-object.md)   
 [Annexe A : Fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md)
