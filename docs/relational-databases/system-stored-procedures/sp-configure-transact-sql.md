---
description: sp_configure (Transact-SQL)
title: sp_configure (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_configure
- sp_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_configure
ms.assetid: d18b251d-b37a-4f5f-b50c-502d689594c8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: af05732674dcf09c6bd828125df5fe4773b62018
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071358"
---
# <a name="sp_configure-transact-sql"></a>sp_configure (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md)]

  Affiche ou modifie des options de configuration générales pour le serveur actif.

> [!NOTE]  
> Pour les options de configuration au niveau de la base de données, consultez [ALTER DATABASE scoped configuration &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Pour configurer soft-NUMA, consultez [Soft-numa &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server  
  
sp_configure [ [ @configname = ] 'option_name'   
    [ , [ @configvalue = ] 'value' ] ]  
```  
  
```  
-- Syntax for Parallel Data Warehouse  
  
-- List all of the configuration options  
sp_configure  
[;]  
  
-- Configure Hadoop connectivity  
sp_configure [ @configname= ] 'hadoop connectivity',  
             [ @configvalue = ] { 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 }  
[;]  
RECONFIGURE  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @configname = ] 'option_name'` Nom d’une option de configuration. *option_name* est **varchar(35)** , avec NULL comme valeur par défaut. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] reconnaît toute chaîne unique qui fait partie du nom de configuration. Si ce dernier n'est pas spécifié, la liste complète des options est renvoyée.  
  
 Pour plus d’informations sur les options de configuration disponibles et leurs paramètres, consultez [options de configuration du serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
`[ @configvalue = ] 'value'` Nouveau paramètre de configuration. *value* est de type **int**, avec NULL comme valeur par défaut. La valeur maximale dépend de l'option individuelle.  
  
 Pour afficher la valeur maximale de chaque option, consultez la colonne **maximum** de l' **sys.config** affichage catalogue urations.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 En cas d’exécution sans paramètre, **sp_configure** retourne un jeu de résultats avec cinq colonnes et trie les options par ordre alphabétique dans l’ordre croissant, comme indiqué dans le tableau suivant.  
  
 Les valeurs de **config_value** et **run_value** ne sont pas équivalentes automatiquement. Après la mise à jour d’un paramètre de configuration à l’aide d' **sp_configure**, l’administrateur système doit mettre à jour la valeur de configuration en cours d’exécution en utilisant RECONFIGURE ou RECONFIGURE WITH OVERRIDE. Pour plus d'informations, consultez la section Notes.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(35)**|Nom de l'option de configuration.|  
|**minimum**|**int**|Valeur minimale de l'option de configuration.|  
|**maximum**|**int**|Valeur maximale de l'option de configuration.|  
|**config_value**|**int**|Valeur à laquelle l’option de configuration a été définie à l’aide de **sp_configure** (valeur dans **sys.configurations. Value**). Pour plus d’informations sur ces options, consultez [options de configuration du serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md) et [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
|**run_value**|**int**|Valeur en cours d’exécution de l’option de configuration (valeur dans **sys.configurations.value_in_use**).<br /><br /> Pour plus d’informations, consultez [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
  
## <a name="remarks"></a>Notes  
 Utilisez **sp_configure** pour afficher ou modifier les paramètres au niveau du serveur. Pour modifier les paramètres au niveau de la base de données, utilisez ALTER DATABASE. Pour modifier uniquement les paramètres qui ont une incidence sur la session de l'utilisateur actuel, utilisez l'instruction SET.  
  
### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="updating-the-running-configuration-value"></a>Mise à jour de la valeur de configuration en cours d'exécution  
 Lorsque vous spécifiez une nouvelle *valeur* pour une *option*, le jeu de résultats affiche cette valeur dans la colonne **config_value** . Cette valeur est initialement différente de la valeur dans la colonne **run_value** , qui affiche la valeur de configuration en cours d’exécution. Pour mettre à jour la valeur de configuration en cours d’exécution dans la colonne **run_value** , l’administrateur système doit exécuter RECONFIGURE ou RECONFIGURE WITH OVERRIDE.  
  
 Ces deux instructions fonctionnent avec toutes les options de configuration. Cependant, l'instruction de base RECONFIGURE rejette toute valeur d'option se trouvant en dehors d'une plage appropriée ou pouvant être à l'origine de conflits parmi les options. Par exemple, reconfigure génère une erreur si la valeur de l' **intervalle de récupération** est supérieure à 60 minutes ou si la valeur du **masque d’affinité** chevauche la valeur **Affinity I/O mask** . Par contre, RECONFIGURE WITH OVERRIDE accepte toutes les valeurs d'option présentant le bon type de données et force la reconfiguration avec la valeur spécifiée.  
  
> [!CAUTION]  
> Une valeur d'option inappropriée peut avoir des répercussions négatives sur la configuration de l'instance de serveur. Utilisez RECONFIGURE WITH OVERRIDE avec prudence.  
  
 Certaines options peuvent être mises à jour de façon dynamique par l'instruction RECONFIGURE, alors que d'autres nécessitent un arrêt et un redémarrage du serveur. Par exemple, les options **min Server Memory** et **Max Server** Memory Server sont mises à jour dynamiquement dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ; par conséquent, vous pouvez les modifier sans redémarrer le serveur. En revanche, la reconfiguration de la valeur d’exécution de l’option **facteur de remplissage** nécessite le redémarrage de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Après l’exécution de RECONFIGURE sur une option de configuration, vous pouvez voir si l’option a été mise à jour de manière dynamique en exécutant **sp_configure «**_option_name_*_»_*. Les valeurs des colonnes **run_value** et **config_value** doivent correspondre pour une option mise à jour dynamiquement. Vous pouvez également vérifier quelles options sont dynamiques en examinant la **is_dynamic** colonne de l’affichage catalogue **sys.configurations** .  
 
 La modification est également écrite dans le journal des erreurs SQL Server.
  
> [!NOTE]  
>  Si une *valeur* spécifiée est trop élevée pour une option, la colonne **run_value** reflète le fait que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a utilisé par défaut la mémoire dynamique plutôt que d’utiliser un paramètre non valide.  
  
 Pour plus d’informations, consultez [REconfigure &#40;&#41;Transact-SQL ](../../t-sql/language-elements/reconfigure-transact-sql.md).  
  
## <a name="advanced-options"></a>Options avancées  
 Certaines options de configuration, telles que le **masque d’affinité** et l' **intervalle de récupération**, sont désignées comme options avancées. Par défaut, ces options ne sont ni affichables ni modifiables. Pour les rendre disponibles, attribuez la valeur 1 à l’option de configuration **afficher les options avancées** . 
 
> [!CAUTION]  
> Lorsque l’option **afficher les options avancées** a la valeur 1, ce paramètre s’applique à tous les utilisateurs. Il est recommandé d’utiliser uniquement cet État temporairement et de revenir à 0 lorsque vous avez terminé avec la tâche qui a requis l’affichage des options avancées.  
  
 Pour plus d’informations sur les options de configuration et leurs paramètres, consultez [options de configuration du serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, vous devez disposer de l’autorisation ALTER Settings au niveau du serveur. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-the-advanced-configuration-options"></a>R. Affichage des options de configuration avancées  
 L'exemple suivant montre comment définir et afficher toutes les options de configuration. Pour afficher les options de configuration avancées, il faut tout d'abord donner à l'argument `show advanced option` la valeur `1`. Une fois cette option modifiée, l'exécution de `sp_configure` sans paramètre renvoie l'ensemble des options de configuration.  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'show advanced option', '1';  
```  
  
 Voici le message : « L'option de configuration 'show advanced options' est passée de 0 à 1. Pour installer, exécutez l'instruction RECONFIGURE. »  
  
 Exécutez `RECONFIGURE` et affichez toutes les options de configuration :  
  
```sql  
RECONFIGURE;  
EXEC sp_configure;  
```  
  
### <a name="b-changing-a-configuration-option"></a>B. Modification d'une option de configuration  
 L'exemple suivant définit la valeur de l'option système `recovery interval` à `3` minutes.  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'recovery interval', '3';  
RECONFIGURE WITH OVERRIDE;  
```  
  
## <a name="examples-sspdw"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-list-all-available-configuration-settings"></a>C. Liste de tous les paramètres de configuration disponibles  
 L'exemple suivant montre comment afficher toutes les options de configuration.  
  
```sql  
EXEC sp_configure;  
```  
  
 Le résultat renvoie le nom de l'option suivi des valeurs minimales et maximales de cette option. Le **config_value** est la valeur qui [!INCLUDE[ssDW](../../includes/ssdw-md.md)] sera utilisée lorsque la reconfiguration est terminée. **run_value** est la valeur en cours d’utilisation. Les valeurs **config_value** et **run_value** sont généralement identiques, sauf si la valeur est en cours de modification.  
  
### <a name="d-list-the-configuration-settings-for-one-configuration-name"></a>D. Liste des paramètres de configuration pour un nom de configuration  
  
```sql  
EXEC sp_configure @configname='hadoop connectivity';  
```  
  
### <a name="e-set-hadoop-connectivity"></a>E. Définir la connectivité Hadoop  
 La définition de la connectivité Hadoop nécessite quelques étapes supplémentaires en plus de l’exécution de sp_configure. Pour obtenir la procédure complète, consultez [Create external data SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
  
  
