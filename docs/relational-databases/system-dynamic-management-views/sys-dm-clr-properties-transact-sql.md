---
description: sys.dm_clr_properties (Transact-SQL)
title: sys.dm_clr_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_clr_properties
- sys.dm_clr_properties_TSQL
- dm_clr_properties_TSQL
- dm_clr_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_properties dynamic management view
ms.assetid: 220d062f-d117-46e7-a448-06fe48db8163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbf92690e35a065afc9aab936e5a8e37ef59f6f6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100352693"
---
# <a name="sysdm_clr_properties-transact-sql"></a>sys.dm_clr_properties (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Renvoie une ligne pour chaque propriété relative à l'intégration du CLR (Common Language Runtime) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comprenant la version et l'état du CLR hébergé. Le CLR hébergé est initialisé en exécutant les instructions [Create Assembly](../../t-sql/statements/create-assembly-transact-sql.md), [ALTER assembly](../../t-sql/statements/alter-assembly-transact-sql.md)ou [Drop assembly](../../t-sql/statements/drop-assembly-transact-sql.md) , ou en exécutant une routine, un type ou un déclencheur CLR. La vue de **sys.dm_clr_properties** ne spécifie pas si l’exécution du code CLR utilisateur a été activée sur le serveur. L’exécution du code CLR utilisateur est activée à l’aide de la procédure stockée [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) avec l’option [CLR activée](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) définie sur 1.  
  
 La vue **sys.dm_clr_properties** contient les colonnes **Name** et **value** . Chaque ligne de cette vue fournit des détails sur une propriété du CLR hébergé. Utilisez cette vue pour recueillir des informations sur le CLR hébergé, telles que le répertoire d'installation du CLR, sa version ou encore son état actuel. Cette vue peut vous aider à déterminer que le code d'intégration CLR ne fonctionne pas en raison de problèmes d'installation du CLR sur le serveur.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Nom de la propriété.|  
|**value**|**nvarchar(128)**|Valeur de la propriété.|  
  
## <a name="properties"></a>Propriétés  
 La propriété **Directory** indique le répertoire dans lequel le .NET Framework a été installé sur le serveur. Il existe plusieurs façons d'installer le .NET Framework sur le serveur et la valeur de cette propriété identifie l'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est utilisée.  
  
 La propriété **version** indique la version du .NET Framework et du CLR hébergé sur le serveur.  
  
 La **sys.dm_clr_properties** vue managée dynamique peut retourner six valeurs différentes pour la propriété **State** , qui reflète l’état du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR hébergé. Il s'agit de :  
  
-   Mscoree is not loaded.  
  
-   Mscoree is loaded.  
  
-   Locked CLR version with mscoree.  
  
-   CLR is initialized.  
  
-   CLR initialization permanently failed.  
  
-   CLR is stopped.  
  
 Le **Mscoree n’est pas chargé** et les États de **chargement de Mscoree** affichent la progression de l’initialisation du CLR hébergé au démarrage du serveur et ne sont pas susceptibles d’être affichés.  
  
 La **version du CLR verrouillé avec** l’État Mscoree peut être affichée là où le CLR hébergé n’est pas utilisé et, par conséquent, il n’a pas encore été initialisé. Le CLR hébergé est initialisé la première fois qu’une instruction DDL (telle que [Create assembly &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)) ou un objet de base de données managé est exécuté.  
  
 L’état initialisé par le **CLR** indique que le CLR hébergé a été correctement initialisé. Cela n'indique pas si l'exécution de code CLR utilisateur a été activée. Si l’exécution du code CLR de l’utilisateur est activée pour la première fois, puis désactivée à l’aide de la [!INCLUDE[tsql](../../includes/tsql-md.md)] procédure stockée [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) , la valeur **d'** État sera toujours CLR.  
  
 L’état **d’échec de l’initialisation du CLR n'** indique pas l’échec de l’initialisation du CLR hébergé. Cela est probablement dû à une insuffisance de mémoire ou à l'échec de la négociation d'hébergement entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR. Si tel est le cas, le message d'erreur 6512 ou 6513 est déclenché.  
  
 L' **État du CLR arrêté** est uniquement visible lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’arrêt.  
  
## <a name="remarks"></a>Notes  
 Les propriétés et les valeurs de cette vue peuvent changer dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en raison des améliorations apportées à la fonctionnalité d’intégration du CLR.  
  
## <a name="permissions"></a>Autorisations  
  
Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="examples"></a>Exemples  
 L'exemple suivant récupère des informations sur le CLR hébergé :  
  
```  
SELECT name, value   
FROM sys.dm_clr_properties;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées au Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
