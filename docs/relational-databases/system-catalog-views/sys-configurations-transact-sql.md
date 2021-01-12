---
description: sys.configurations (Transact-SQL)
title: sys.configurations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.configurations_TSQL
- configurations
- sys.configurations
- configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.configurations catalog view
ms.assetid: c4709ed1-bf88-4458-9e98-8e9b78150441
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 10956e1c3738c64829ec18d92a05b9f427b1833d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100675"
---
# <a name="sysconfigurations-transact-sql"></a>sys.configurations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque valeur de l’option de configuration au niveau du serveur dans le système.  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|ID unique pour la valeur de configuration.|  
|**name**|**nvarchar(35)**|Nom de l'option de configuration.|  
|**value**|**sql_variant**|Valeur configurée pour cette option.|  
|**minimum**|**sql_variant**|Valeur minimale pour l'option de configuration.|  
|**maximum**|**sql_variant**|Valeur maximale pour l'option de configuration.|  
|**value_in_use**|**sql_variant**|Valeur en cours d'exécution actuellement en effet pour cette option.|  
|**description**|**nvarchar(255)**|Description de l'option de configuration.|  
|**is_dynamic**|**bit**|1 = Variable qui prend effet lorsque l'instruction RECONFIGURE est exécutée.|  
|**is_advanced**|**bit**|1 = la variable s’affiche uniquement lorsque l' **affichage advancedoption** est défini.|  
  
 ## <a name="remarks"></a>Notes
  Pour obtenir la liste de toutes les options de configuration du serveur, consultez [options de configuration du serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
> [!NOTE]  
>  Pour les options de configuration au niveau de la base de données, consultez [ALTER DATABASE scoped configuration &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Pour configurer soft-NUMA, consultez [Soft-numa &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
 
L’affichage catalogue sys.configurations peut être utilisé pour déterminer le config_value (colonne valeur), le run_value (colonne value_in_use) et si l’option de configuration est dynamique (ne nécessite pas de redémarrage du moteur du serveur ou la colonne is_dynamic).

> [!NOTE]
> Le config_value dans le jeu de résultats de sp_configure est équivalent à la colonne **sys.configurations. Value** . Le **run_value** est équivalent au **sys.configurations.value_in_use** colonne.

La requête suivante peut être utilisée pour déterminer si des valeurs configurées n’ont pas été installées :

```SQL
select * from sys.configurations where value != value_in_use
```

Si la valeur est égale à la modification de l’option de configuration que vous avez effectuée mais que le **value_in_use** n’est pas le même, soit la commande reconfigure n’a pas été exécutée, soit elle a échoué, soit le moteur du serveur doit être redémarré.

Il existe des options de configuration dans lesquelles la valeur et la value_in_use peuvent ne pas être identiques, ce qui correspond au comportement attendu. Par exemple :

« Max Server Memory (Mo) »-la valeur configurée par défaut de 0 apparaît comme **value_in_use** = 2147483647<br>

« min Server Memory (Mo) »-la valeur configurée par défaut de 0 peut s’afficher comme **value_in_use** = 8 (32 bits) ou 16 (64 bits). Dans certains cas, le **value_in_use** est égal à 0. Dans ce cas, la **value_in_use** « true » est de 8 (32 bits) ou 16 (64 bits).


La colonne **is_dynamic** peut être utilisée pour déterminer si l’option de configuration nécessite un redémarrage. is_dynamic = 1 signifie que lorsque la commande reconfigure (T-SQL) est exécutée, la nouvelle valeur prend effet « immédiatement » (dans certains cas, le moteur du serveur peut ne pas évaluer la nouvelle valeur immédiatement, mais le fera dans le cours normal de son exécution). is_dynamic = 0 signifie que la valeur de configuration modifiée ne prendra effet qu’après le redémarrage du serveur, même si la commande reconfigure (T-SQL) a été exécutée.

Pour une option de configuration qui n’est pas dynamique, il n’existe aucun moyen de savoir si la commande reconfigure (T-SQL) a été exécutée pour effectuer la première étape de l’installation de la modification de configuration. Avant de redémarrer SQL Server pour installer une modification de configuration, exécutez la commande reconfigure (T-SQL) pour vous assurer que toutes les modifications de configuration prennent effet après un redémarrage du SQL Server. 
 
 
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de configurations à l’ensemble du serveur &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
