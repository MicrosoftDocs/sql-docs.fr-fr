---
description: Informations sur les erreurs liées aux champs
title: Informations sur l’erreur de Field-Related | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- field-related errors [ADO]
- errors [ADO], field-related
ms.assetid: 5e7b1af4-996b-47c5-9161-c5575ad4fec9
author: rothja
ms.author: jroth
ms.openlocfilehash: cce74cf105aa7b38c2a3d7b157ef0fa17a1e8c1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037469"
---
# <a name="field-related-error-information"></a>Informations sur les erreurs liées aux champs
Si une erreur est directement liée à un champ (par exemple, si les données sont manquantes ou si le type est incorrect pour le champ), vous pouvez récupérer des informations supplémentaires sur la cause du problème en examinant la propriété **Status** de l’objet **Field** . Cette propriété a été améliorée pour fournir des informations spécifiques sur le problème. Ainsi, par exemple, lorsqu’un appel à **UpdateBatch** échoue, la cause du problème peut être déterminée en examinant la propriété **Status** des **champs** de chacun des enregistrements concernés. La propriété contient l’une des valeurs de la constante **FieldStatusEnum** . Le tableau suivant répertorie les valeurs qui présentent un intérêt particulier lorsqu’une erreur se produit.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adFieldCantConvertValue**|2|Indique que le champ ne peut pas être récupéré ou stocké sans perte de données.|  
|**adFieldDataOverflow**|6|Indique que les données retournées par le fournisseur ont débordé le type de données du champ.|  
|**adFieldDefault**|13|Indique que la valeur par défaut du champ a été utilisée lors de la définition des données.|  
|**adFieldIgnore**|15|Indique que ce champ a été ignoré lors de la définition des valeurs de données dans la source. Aucune valeur n’a été définie par le fournisseur.|  
|**adFieldIntegrityViolation**|10|Indique que le champ ne peut pas être modifié, car il s’agit d’une entité calculée ou dérivée.|  
|**adFieldIsNull**|3|Indique que le fournisseur a retourné une valeur null.|  
|**adFieldOutOfSpace**|22|Indique que le fournisseur ne peut pas obtenir suffisamment d’espace de stockage pour effectuer une opération de déplacement ou de copie.|
