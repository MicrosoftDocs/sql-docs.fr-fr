---
description: catalog.event_message_context
title: catalog.event_message_context | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 273a54f8-b107-4f36-9461-2b475644760d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 880f17c309718522d589b0fea453d490e7d01b3f
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835256"
---
# <a name="catalogevent_message_context"></a>catalog.event_message_context 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche des informations sur les conditions associées aux messages d'événements d'exécution, pour les exécutions sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Context_id|bigint|ID unique du contexte de l'erreur.|  
|Event_message_id|bigint|ID unique du message auquel le contexte est associé.|  
|Context_depth|int|À mesure que la profondeur augmente, le contexte s'éloigne de l'erreur. Lorsqu'une erreur se produit, la profondeur du contexte commence à 1. La valeur 0 indique l'état du package avant le démarrage de l'exécution.|  
|Package_path|Nvarchar(max)|Chemin d'accès au package de la source du contexte.|  
|Context_type|SMALLINT|Type de l'objet qui est la source du contexte. Consultez la section **Remarques** pour obtenir la liste des types de contexte.|  
|Context_source_name|Nvarchar(4000)|Nom de l'objet qui est la source du contexte.|  
|Context_source_id|Nvarchar(38)|ID unique de l'objet qui est la source du contexte.|  
|Property_name|Nvarchar(4000)|Nom de la propriété associée à la source du contexte.|  
|Property_value|Sql_variant|Valeur de la propriété associée à la source du contexte.|  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant répertorie les types de contexte.  
  
|Valeur du type de contexte|Nom de type|Description|  
|-|-|-|  
|10|Tâche|État d'une tâche lorsqu'une erreur s'est produite.|  
|20|Pipeline|L'erreur provient d'un composant de pipeline : source, destination ou composant de transformation.|  
|30|Séquence|État d'une séquence.|  
|40|Boucle For|État d'une boucle For.|  
|50|Boucle Foreach|État d'une boucle Foreach.|  
|60|Paquet|État du package lorsqu'une erreur s'est produite.|  
|70|Variable|Valeur de variable|  
|80|Gestionnaire de connexions|Propriétés d'un gestionnaire de connexions.|  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'opération  
  
-   Appartenance au rôle de base de données **ssis_admin**.  
  
-   Appartenance au rôle serveur **sysadmin**.  
  
  
