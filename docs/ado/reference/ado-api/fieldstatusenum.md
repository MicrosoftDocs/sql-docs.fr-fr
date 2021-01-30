---
description: FieldStatusEnum
title: FieldStatusEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldStatusEnum
helpviewer_keywords:
- FieldStatusEnum enumeration [ADO]
ms.assetid: e06da1e2-303f-41b2-a3b0-61e233da152c
author: rothja
ms.author: jroth
ms.openlocfilehash: 968a4205a12e9f6ff36c3bff512cf41ae60ca469
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167347"
---
# <a name="fieldstatusenum"></a>FieldStatusEnum
Spécifie l' [État](./status-property-ado-field.md) d’un [objet de champ](./field-object.md).  
  
 Les valeurs ** \* adFieldPending* _ indiquent l’opération qui a provoqué la définition de l’État et peuvent être combinées avec d’autres valeurs d’État.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|_ *adFieldAlreadyExists**|26|Indique que le champ spécifié existe déjà.|  
|**adFieldBadStatus**|12|Indique qu’une valeur d’État non valide a été envoyée à partir d’ADO au fournisseur OLE DB. Les causes possibles incluent un fournisseur OLE DB 1,0 ou 1,1, ou une combinaison incorrecte de [valeur](./value-property-ado.md) et d' [État](./status-property-ado-field.md).|  
|**adFieldCannotComplete**|20|Indique que le serveur de l’URL spécifiée par la [source](./source-property-ado-record.md) n’a pas pu terminer l’opération.|  
|**adFieldCannotDeleteSource**|23|Indique qu’au cours d’une opération de déplacement, une arborescence ou une sous-arborescence a été déplacée vers un nouvel emplacement, mais la source n’a pas pu être supprimée.|  
|**adFieldCantConvertValue**|2|Indique que le champ ne peut pas être récupéré ou stocké sans perte de données.|  
|**adFieldCantCreate**|7|Indique que le champ n’a pas pu être ajouté, car le fournisseur a dépassé une limitation (par exemple, le nombre de champs autorisés).|  
|**adFieldDataOverflow**|6|Indique que les données retournées par le fournisseur ont débordé le type de données du champ.|  
|**adFieldDefault**|13|Indique que la valeur par défaut du champ a été utilisée lors de la définition des données.|  
|**adFieldDoesNotExist**|16|Indique que le champ spécifié n’existe pas.|  
|**adFieldIgnore**|15|Indique que ce champ a été ignoré lors de la définition des valeurs de données dans la source. Le fournisseur n’a pas de valeur.|  
|**adFieldIntegrityViolation**|10|Indique que le champ ne peut pas être modifié, car il s’agit d’une entité calculée ou dérivée.|  
|**adFieldInvalidURL**|17|Indique que l’URL de la source de données contient des caractères non valides.|  
|**adFieldIsNull**|3|Indique que le fournisseur a retourné une valeur de type VARIANT de type VT_NULL et que le champ n’est pas vide.|  
|**adFieldOK**|0|Par défaut. Indique que le champ a été ajouté ou supprimé.|  
|**adFieldOutOfSpace**|22|Indique que le fournisseur ne peut pas obtenir suffisamment d’espace de stockage pour effectuer une opération de déplacement ou de copie.|  
|**adFieldPendingChange**|0x40000|Indique soit que le champ a été supprimé, puis rajouté, peut-être avec un type de données différent, soit que la valeur du champ qui avait précédemment un état **adFieldOK** a changé. La forme finale du champ modifie la collection [Fields](./fields-collection-ado.md) après l’appel de la méthode [Update](./update-method.md) .|  
|**adFieldPendingDelete**|0x20000|Indique que l’opération de **suppression** a entraîné la définition de l’État. Le champ a été marqué pour être supprimé de la collection **Fields** après l’appel de la méthode **Update** .|  
|**adFieldPendingInsert**|0x10000|Indique que l’opération d' **Ajout** a entraîné la définition de l’État. Le **champ** a été marqué pour être ajouté à la collection de **champs** après l’appel de la méthode **Update** .|  
|**adFieldPendingUnknown**|0x80000|Indique que le fournisseur ne peut pas déterminer l’opération qui a provoqué la définition de l’état du champ.|  
|**adFieldPendingUnknownDelete**|0x100000|Indique que le fournisseur ne peut pas déterminer quelle opération a provoqué la définition de l’état du champ et que le champ sera supprimé de la collection de **champs** après l’appel de la méthode de **mise à jour** .|  
|**adFieldPermissionDenied**|9|Indique que le champ ne peut pas être modifié, car il est défini en lecture seule.|  
|**adFieldReadOnly**|24|Indique que le champ de la source de données est défini en lecture seule.|  
|**adFieldResourceExists**|19|Indique que le fournisseur n’a pas pu effectuer l’opération, car il existe déjà un objet sur l’URL de destination et il n’est pas en mesure de remplacer l’objet.|  
|**adFieldResourceLocked**|18|Indique que le fournisseur n’a pas pu effectuer l’opération, car la source de données est verrouillée par une ou plusieurs autres applications ou processus.|  
|**adFieldResourceOutOfScope**|25|Indique qu’une URL source ou de destination est en dehors de l’étendue de l’enregistrement en cours.|  
|**adFieldSchemaViolation**|11|Indique que la valeur a violé la contrainte de schéma de source de données pour le champ.|  
|**adFieldSignMismatch**|5|Indique que la valeur de données retournée par le fournisseur a été signée, mais que le type de données de la valeur de champ ADO n’était pas signé.|  
|**adFieldTruncated**|4|Indique que les données de longueur variable ont été tronquées lors de la lecture de la source de données.|  
|**adFieldUnavailable**|8|Indique que le fournisseur n’a pas pu déterminer la valeur lors de la lecture de la source de données. Par exemple, la ligne vient d’être créée, la valeur par défaut pour la colonne n’est pas disponible et une nouvelle valeur n’a pas encore été spécifiée.|  
|**adFieldVolumeNotFound**|21|Indique que le fournisseur ne parvient pas à localiser le volume de stockage indiqué par l’URL.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [Status, propriété (objet Field ADO)](./status-property-ado-field.md)