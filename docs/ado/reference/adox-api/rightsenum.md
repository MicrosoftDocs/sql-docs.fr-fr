---
description: RightsEnum
title: RightsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RightsEnum
helpviewer_keywords:
- RightsEnum enumeration [ADOX]
ms.assetid: 55ee67c7-a583-42aa-849a-78264b4cb614
author: rothja
ms.author: jroth
ms.openlocfilehash: d89daeb8d85e9e59e60f6a1aa6b918364ab915fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169215"
---
# <a name="rightsenum"></a>RightsEnum
Spécifie les droits ou les autorisations pour un groupe ou un utilisateur sur un objet.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adRightCreate**|16384 (&H4000)|L’utilisateur ou le groupe a l’autorisation de créer des objets de ce type.|  
|**adRightDelete**|65536 (&H10000)|L’utilisateur ou le groupe a l’autorisation de supprimer des données d’un objet. Pour les objets tels que les **tables**, l’utilisateur a l’autorisation de supprimer des valeurs de données des enregistrements.|  
|**adRightDrop**|256 (&H100)|L’utilisateur ou le groupe a l’autorisation de supprimer des objets du catalogue. Par exemple, les **tables** peuvent être supprimées par une commande SQL DROP TABLE.|  
|**adRightExclusive**|512 (&H200)|L’utilisateur ou le groupe a l’autorisation d’accéder à l’objet en mode exclusif.|  
|**adRightExecute**|536870912 (&H20000000)|L’utilisateur ou le groupe a l’autorisation d’exécuter l’objet.|  
|**adRightFull**|268435456 (&H10000000)|L’utilisateur ou le groupe dispose de toutes les autorisations sur l’objet.|  
|**adRightInsert**|32768 (&H8000)|L’utilisateur ou le groupe a l’autorisation d’insérer l’objet. Pour les objets tels que les **tables**, l’utilisateur a l’autorisation d’insérer des données dans la table.|  
|**adRightMaximumAllowed**|33554432 (&H2000000)|L’utilisateur ou le groupe a le nombre maximal d’autorisations autorisées par le fournisseur. Les autorisations spécifiques sont dépendantes du fournisseur.|  
|**adRightNone**|0|L’utilisateur ou le groupe n’a pas d’autorisations pour l’objet.|  
|**adRightRead**|-2147483648 (&H80000000)|L’utilisateur ou le groupe a l’autorisation de lire l’objet. Pour les objets tels que les [tables](./table-object-adox.md), l’utilisateur a l’autorisation de lire les données de la table.|  
|**adRightReadDesign**|1024 (&H400)|L’utilisateur ou le groupe a l’autorisation de lire la conception de l’objet.|  
|**adRightReadPermissions**|131072 (&H20000)|L’utilisateur ou le groupe peut afficher, mais pas modifier, les autorisations spécifiques pour un objet dans le catalogue.|  
|**adRightReference**|8192 (&H2000)|L’utilisateur ou le groupe a l’autorisation de faire référence à l’objet.|  
|**adRightUpdate**|1073741824 (&H40000000)|L’utilisateur ou le groupe a l’autorisation de mettre à jour l’objet. Pour les objets tels que les **tables**, l’utilisateur a l’autorisation de mettre à jour les données de la table.|  
|**adRightWithGrant**|4096 (&H1000)|L’utilisateur ou le groupe a l’autorisation d’accorder des autorisations sur l’objet.|  
|**adRightWriteDesign**|2048 (&H800)|L’utilisateur ou le groupe a l’autorisation de modifier la conception de l’objet.|  
|**adRightWriteOwner**|524288 (&H80000)|L’utilisateur ou le groupe a l’autorisation de modifier le propriétaire de l’objet.|  
|**adRightWritePermissions**|262144 (&H40000)|L’utilisateur ou le groupe peut modifier les autorisations spécifiques d’un objet dans le catalogue.|  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [GetPermissions, méthode (ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [SetPermissions, méthode (ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::