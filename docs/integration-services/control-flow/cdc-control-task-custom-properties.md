---
description: Propriétés personnalisées de la tâche de contrôle de capture de données modifiées
title: Propriétés personnalisées de la tâche de contrôle de capture de données modifiées | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2a073699-79a2-4ea1-a68e-fc17a80b74ba
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cac3695cd99d5065862604a77a986921568ac292
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049126"
---
# <a name="cdc-control-task-custom-properties"></a>Propriétés personnalisées de la tâche de contrôle de capture de données modifiées

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le tableau suivant décrit les propriétés personnalisées de la tâche de contrôle de capture de données modifiées. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|Connexion|Connexion ADO.NET|Connexion ADO.NET à la base de données CDC [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] pour l’accès aux tables de modifications et à l’état de capture des données modifiées, si elles sont stockées dans la même base de données.<br /><br /> La connexion doit être établie avec une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] activée pour la capture de données modifiées et dans laquelle la table de modifications sélectionnée est localisée.|  
|TaskOperation|Integer (énumération)|Opération sélectionnée pour la tâche de contrôle de capture de données modifiées. Les valeurs possibles sont **Marquer le début de la charge initiale**, **Marquer la fin de la charge initiale**, **Marquer le début de la capture de données modifiées**, **Obtenir la plage de traitement**, **Marquer la plage traitée** et **Rétablir l’état de la capture de données modifiées**.<br /><br /> Si vous sélectionnez **MarkCdcStart**, **MarkInitialLoadStart** ou **MarkInitialLoadEnd** quand vous travaillez sur la capture de données modifiées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (autrement dit, hors d’Oracle), l’utilisateur spécifié dans le gestionnaire de connexions doit être  **db_owner** ou **sysadmin**.<br /><br /> Pour plus d’informations sur ces opérations, consultez [Éditeur de tâche de contrôle CDC](./cdc-control-task.md) et [Tâche de contrôle de capture de données modifiées](../../integration-services/control-flow/cdc-control-task.md).|  
|OperationParameter|String|Actuellement utilisé avec l’opération **MarkCdcStart** . Ce paramètre permet d'effectuer une entrée supplémentaire requise pour l'opération spécifique. Par exemple, le NSE nécessaire pour l’opération **MarkCdcStart** .|  
|StateVariable|String|Variable de package SSIS qui stocke l'état de capture de données modifiées du contexte de capture de données modifiées actuel. La tâche de contrôle de capture de données modifiées lit et écrit l’état dans **StateVariable** et ne le charge pas (pas plus qu’elle ne le stocke) dans un stockage permanent, sauf si **AutomaticStatePersistence** est sélectionné. Voir [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md).|  
|AutomaticStatePersistence|Boolean|La tâche de contrôle de capture de données modifiées lit l'état de capture de données modifiées dans la variable de package d'état de capture de données modifiées. Après une opération, la tâche de contrôle de capture de données modifiées met à jour la valeur de la variable de package d'état de capture de données modifiées. La propriété **AutomaticStatePersistence** indique à la tâche de contrôle de capture de données modifiées la personne chargée de rendre la valeur d’état de capture de données modifiées permanente entre les séries de package SSIS.<br /><br /> Quand cette propriété a la valeur **true**, la tâche de contrôle de capture de données modifiées charge automatiquement la valeur de la variable d’état de capture de données modifiées à partir d’une table d’état. Quand la tâche de contrôle de capture de données modifiées met à jour la valeur de la variable d’état de capture de données modifiées, elle met également à jour sa valeur dans le même état **table.stores**, l’état dans une table spéciale et met à jour la variable d’état. Le développeur peut contrôler la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient cette table d'état et son nom. la structure de cette table d'état est prédéfinie.<br /><br /> Si la valeur est **false**, la tâche de contrôle de capture de données modifiées ne se charge pas de rendre sa valeur permanente. Si la valeur est true, la tâche de contrôle de capture de données modifiées stocke l'état dans une table spéciale et met à jour StateVariable.<br /><br /> La valeur par défaut est **true**, ce qui indique que la permanence de l’état est mise à jour automatiquement.|  
|StateConnection|Connexion ADO.NET|Connexion ADO.NET à la base de données dans laquelle la table d’état réside en cas d’utilisation **d’AutomaticStatePersistence**. La valeur par défaut est la même que pour **Connexion**.|  
|StateName|String|Nom associé à l'état permanent. La pleine charge et les packages de capture de données modifiées qui fonctionnent avec le même contexte de capture de données modifiées spécifient un nom de contexte de capture de données modifiées commun. Ce nom est utilisé pour surveiller la ligne d’état dans la table d’état.<br /><br /> Cette propriété s’applique uniquement quand **AutomaticStatePersistence** a la valeur **true**.|  
|StateTable|String|Spécifie le nom de la table dans laquelle l'état de contexte de capture de données modifiées est stocké. Cette table doit être accessible à l'aide de la connexion configurée pour ce composant. Cette table doit inclure des colonnes varchar appelées **nom** et **état**. (La colonne **état** doit comporter au moins 256 caractères.)<br /><br /> Cette propriété s’applique uniquement quand **AutomaticStatePersistence** a la valeur **true**.|  
|CommandTimeout|entier|Cette valeur indique le délai d’attente (en secondes) à utiliser pour communiquer avec la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette valeur est utilisée lorsque le temps de réponse de la base de données est très lent et que la valeur par défaut (30 secondes) n’est pas suffisante.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tâche de contrôle de capture de données modifiées](../../integration-services/control-flow/cdc-control-task.md)   
 [Éditeur de tâche de contrôle CDC](./cdc-control-task.md)  
  
