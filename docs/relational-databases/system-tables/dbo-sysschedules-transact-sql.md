---
description: dbo.sysschedules (Transact-SQL)
title: Planifications de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysschedules_TSQL
- sysschedules
- sysschedules_TSQL
- dbo.sysschedules
dev_langs:
- TSQL
helpviewer_keywords:
- sysschedules system table
ms.assetid: 4cac9237-7a69-4035-bb3e-928b76aad698
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9c0bb164d77ba4c505207e56897ecc17f8644821
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545805"
---
# <a name="dbosysschedules-transact-sql"></a>dbo.sysschedules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur les planifications de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette table est stockée dans la base de données **msdb** .  
  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|ID de la planification de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**schedule_uid**|**uniqueidentifier**|Identificateur unique de la planification du travail. Cette valeur permet d'identifier une planification pour les travaux distribués.|  
|**originating_server_id**|**int**|ID du serveur principal duquel provient la planification du travail.|  
|**name**|**sysname (nvarchar (128))**|Nom de la planification du travail défini par l'utilisateur. Ce nom doit être unique au sein d'un travail.|  
|**owner_sid**|**varbinary(85)**|Microsoft Windows *security_identifier* de l’utilisateur ou du groupe propriétaire de la planification du travail.|  
|**activé**|**int**|État de la planification du travail :<br /><br /> **0** = non activé.<br /><br /> **1** = activé.<br /><br /> Si la planification n'est pas activée, aucun travail n'est exécuté sur la planification.|  
|**freq_type**|**int**|Fréquence d'exécution d'un travail pour cette planification.<br /><br /> **1** = une seule fois<br /><br /> **4** = tous les jours<br /><br /> **8** = hebdomadaire<br /><br /> **16** = mensuelle<br /><br /> **32** = mensuelle, par rapport à **freq_interval**<br /><br /> **64** = s’exécute au démarrage du service SQL Server Agent<br /><br /> **128** = s’exécute lorsque l’ordinateur est inactif|  
|**freq_interval**|**int**|Jours d'exécution du travail. Dépend de la valeur de **freq_type**. La valeur par défaut est **0**, ce qui indique que **freq_interval** n’est pas utilisé. Consultez le tableau ci-dessous pour connaître les valeurs possibles et leurs effets.|  
|**freq_subday_type**|**int**|Unités pour le **freq_subday_interval**. Voici les valeurs possibles et leurs descriptions.<br /><br /> <br /><br /> **1** : à l’heure spécifiée<br /><br /> **2** : secondes<br /><br /> **4** : minutes<br /><br /> **8** : heures|  
|**freq_subday_interval**|**int**|Nombre de **freq_subday_type** périodes entre chaque exécution du travail.|  
|**freq_relative_interval**|**int**|Lorsque **freq_interval** se produit chaque mois, si **freq_type** est **32** (mensuel relatif). Il peut s'agir de l'une des valeurs suivantes :<br /><br /> **0**  =  **freq_relative_interval** n’est pas utilisé<br /><br /> **1** = premier<br /><br /> **2** = seconde<br /><br /> **4** = troisième<br /><br /> **8** = quatrième<br /><br /> **16** = dernier|  
|**freq_recurrence_**<br /><br /> **factorisés**|**int**|Nombre de semaines ou de mois entre l’exécution planifiée d’un travail. **freq_recurrence_factor** est utilisé uniquement si **freq_type** est **8**, **16**ou **32**. Si cette colonne contient **0**, **freq_recurrence_factor** n’est pas utilisé.|  
|**active_start_date**|**int**|Date de démarrage de l'exécution d'un travail. La date est au format AAAAMMJJ. NULL indique la date du jour.|  
|**active_end_date**|**int**|Date d'arrêt de l'exécution d'un travail. La date se présente sous la forme AAAAMMJJ.|  
|**active_start_time**|**int**|Heure à partir de n’importe quel jour entre **active_start_date** et **active_end_date** que l’exécution du travail commence. L'heure est au format HHMMSS, exprimée sur 24 h.|  
|**active_end_time**|**int**|Heure à n’importe quel jour entre **active_start_date** et **active_end_date** que l’exécution du travail s’arrête. L'heure est au format HHMMSS, exprimée sur 24 h.|  
|**date_created**|**datetime**|Date et heure de création de la planification.|  
|**date_modified**|**datetime**|Date et heure de dernière modification de la planification.|  
|**version_number**|**int**|Numéro de version en cours de la planification. Par exemple, si une planification a été modifiée 10 fois, le **version_number** est 10.|  
  
|Valeur de freq_type|Effet sur freq_interval|  
|-------------------------|------------------------------|  
|**1** (une fois)|**freq_interval** n’est pas utilisé (**0**)|  
|**4** (tous les jours)|Tous les **freq_interval** jours|  
|**8** (hebdomadaire)|**freq_interval** est un ou plusieurs des éléments suivants :<br /><br /> **1** = dimanche<br /><br /> **2** = lundi<br /><br /> **4** = mardi<br /><br /> **8** = mercredi<br /><br /> **16** = jeudi<br /><br /> **32** = vendredi<br /><br /> **64** = samedi|  
|**16** (mensuellement)|Le **freq_interval** jour du mois|  
|**32** (mensuelle, relative)|**freq_interval** est l’un des éléments suivants :<br /><br /> **1** = dimanche<br /><br /> **2** = lundi<br /><br /> **3** = mardi<br /><br /> **4** = mercredi<br /><br /> **5** = jeudi<br /><br /> **6** = vendredi<br /><br /> **7** = samedi<br /><br /> **8** = jour<br /><br /> **9** = jour de semaine<br /><br /> **10** = jour de week-end|  
|**64** (démarre au démarrage du service SQL Server Agent)|**freq_interval** n’est pas utilisé (**0**)|  
|**128** (s’exécute lorsque l’ordinateur est inactif)|**freq_interval** n’est pas utilisé (**0**)|  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.sysJobSchedules &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
  
  
