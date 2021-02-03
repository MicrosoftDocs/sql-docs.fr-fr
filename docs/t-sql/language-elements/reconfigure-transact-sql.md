---
description: RECONFIGURE (Transact-SQL)
title: RECONFIGURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/20/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RECONFIGURE
- RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- reconfiguring configuration options
- configuration options [SQL Server], reconfiguring
- updating configuration options
- RECONFIGURE, RECONFIGURE statement
- RECONFIGURE
- RECONFIGURE, WITH OVERRIDE statement
ms.assetid: 2e6e4eeb-b70b-4f45-a253-28ac4e595d75
author: cawrites
ms.author: chadam
ms.openlocfilehash: b920a123f104e02b2906d6f55d181bdedbdd1f37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182262"
---
# <a name="reconfigure-transact-sql"></a>RECONFIGURE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour la valeur actuellement configurée (la colonne **config_value** de l’ensemble de résultats de **sp_configure**) d’une option de configuration modifiée par la procédure stockée système **sp_configure**. Dans la mesure où certaines options de configuration exigent l’arrêt puis le redémarrage du serveur pour que la valeur en cours d’exécution soit mise à jour, RECONFIGURE n’actualise pas toujours cette dernière (colonne **run_value** de l’ensemble de résultats de **sp_configure**) lorsqu’une valeur de configuration est modifiée.    
    
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Syntaxe    
    
```syntaxsql
RECONFIGURE [ WITH OVERRIDE ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 RECONFIGURE    
 Indique que, si un paramètre de configuration n'exige pas l'arrêt puis le redémarrage du serveur, la valeur en cours d'exécution doit être mise à jour. RECONFIGURE vérifie également que les nouvelles valeurs de configuration ne contiennent pas de valeurs non valides (par exemple une valeur d’ordre de tri qui n’existe pas dans **syscharsets**) ni de valeurs déconseillées. Dans le cas d'options de configuration n'exigeant pas l'arrêt et le redémarrage du serveur, la valeur en cours d'exécution et les valeurs actuellement configurées de l'option de configuration doivent être identiques après la spécification de RECONFIGURE.    
    
 WITH OVERRIDE    
 Désactive la vérification des valeurs de configuration (recherche de valeurs non valides ou déconseillées) pour l’option de configuration avancée **recovery interval**.    
    
 Presque toute option de configuration peut être reconfigurée en utilisant l’option WITH OVERRIDE, à l’exception toutefois de certaines erreurs irrécupérables. Par exemple, vous ne pouvez pas configurer l’option de configuration **min server memory** avec une valeur supérieure à celle spécifiée dans l’option de configuration **max server memory**.
      
## <a name="remarks"></a>Remarques    
 **sp_configure** n’accepte pas de nouvelles valeurs n’appartenant pas à la plage des valeurs autorisées pour chaque option de configuration.    
    
 RECONFIGURE n'est pas autorisée dans une transaction explicite ou implicite. Lorsque vous reconfigurez plusieurs options simultanément, si l'une des opérations de reconfiguration échoue, aucune des opérations de reconfiguration ne prend effet.    
    
 Lors de la reconfiguration de Resource Governor, consultez l’option RECONFIGURE de [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md).    
    
## <a name="permissions"></a>Autorisations    
 Par défaut, les autorisations RECONFIGURE sont accordées aux personnes qui bénéficient de l'autorisation ALTER SETTINGS. Seuls les rôles serveur fixes **sysadmin** et **serveradmin** détiennent implicitement cette autorisation.    
    
## <a name="examples"></a>Exemples    
 L'exemple suivant affecte la valeur `recovery interval` minutes à la limite supérieure de l'option de configuration `75` et utilise `RECONFIGURE WITH OVERRIDE` pour l'installer. Les intervalles de récupération supérieurs à 60 minutes sont déconseillés et désactivés par défaut. Toutefois, étant donné que l'option `WITH OVERRIDE` est spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne vérifie pas si la valeur spécifiée (`75`) est valide pour l'option de configuration `recovery interval`.    
    
```sql    
EXEC sp_configure 'recovery interval', 75    
RECONFIGURE WITH OVERRIDE;    
GO    
```    
    
## <a name="see-also"></a>Voir aussi    
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)     
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)    
    
  
