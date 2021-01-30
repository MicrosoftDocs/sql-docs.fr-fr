---
description: sp_syscollector_create_collection_set (Transact-SQL)
title: sp_syscollector_create_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collection_set_TSQL
- sp_syscollector_create_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_create_collection_set
ms.assetid: 69e9ff0f-c409-43fc-89f6-40c3974e972c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: de9ee3331621df81739a9f808bbd63ab42c52491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205073"
---
# <a name="sp_syscollector_create_collection_set-transact-sql"></a>sp_syscollector_create_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un jeu d'éléments de collecte. Vous pouvez utiliser cette procédure stockée afin de créer un jeu d'éléments de collecte personnalisé pour la collecte de données.  
  
> [!WARNING]  
>  Dans les cas où le compte Windows configuré en tant que proxy est non interactif ou correspond à un utilisateur interactif qui ne s'est pas encore connecté, le répertoire de profils n'existera pas et la création du répertoire intermédiaire échouera. Par conséquent, si vous utilisez un compte proxy sur un contrôleur de domaine, vous devez spécifier un compte interactif qui a été utilisé au moins une fois afin de garantir que le répertoire de profils a été créé.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_create_collection_set   
      [ @name = ] 'name'  
    , [ [ @target = ] 'target' ]  
    , [ [ @collection_mode = ] collection_mode ]  
    , [ [ @days_until_expiration = ] days_until_expiration ]  
    , [ [ @proxy_id = ] proxy_id ]  
    , [ [ @proxy_name = ] 'proxy_name' ]  
    , [ [ @schedule_uid = ] 'schedule_uid' ]  
    , [ [ @schedule_name = ] 'schedule_name' ]  
    , [ [ @logging_level = ] logging_level ]  
    , [ [ @description = ] 'description' ]  
    , [ @collection_set_id = ] collection_set_id OUTPUT   
    , [ [ @collection_set_uid = ] 'collection_set_uid' OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'name'` Nom du jeu d’entités de collecte. *Name* est de **type sysname** et ne peut pas être une chaîne vide ou avoir la valeur null.  
  
 le *nom* doit être unique. Pour obtenir une liste de noms de jeux d'éléments de collecte actuels, interrogez la vue système syscollector_collection_sets.  
  
`[ @target = ] 'target'` Réservé pour une utilisation ultérieure. *Name* est de type **nvarchar (128)** avec NULL comme valeur par défaut.  
  
`[ @collection_mode = ] collection_mode` Spécifie la manière dont les données sont collectées et stockées. *collection_mode* est de type **smallint** et peut avoir l’une des valeurs suivantes :  
  
 0 - Mode mis en cache. La collecte et le téléchargement de données sont sur des planifications séparées. Spécifiez le mode mis en cache pour la collecte continue.  
  
 1 - Mode non mis en cache. La collecte et le téléchargement de données sont sur la même planification. Spécifiez le mode non mis en cache pour une collecte ad hoc ou par instantané.  
  
 La valeur par défaut de *collection_mode* est 0. Lorsque *collection_mode* a la valeur 0, *schedule_uid* ou *schedule_name* doit être spécifié.  
  
`[ @days_until_expiration = ] days_until_expiration` Nombre de jours pendant lesquels les données collectées sont enregistrées dans l’entrepôt de données de gestion. *days_until_expiration* est de type **smallint** avec une valeur par défaut de 730 (deux ans). *days_until_expiration* doit être égal à 0 ou un entier positif.  
  
`[ @proxy_id = ] proxy_id` Identificateur unique d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte proxy de l’agent. *proxy_id* est de **type int** avec NULL comme valeur par défaut. S’il est spécifié, *proxy_name* doit avoir la valeur null. Pour obtenir *proxy_id*, interrogez la table système sysproxies. Le rôle de base de données fixe dc_admin doit avoir l'autorisation d'accéder au proxy. Pour plus d’informations, consultez [créer un Proxy SQL Server Agent](../../ssms/agent/create-a-sql-server-agent-proxy.md).  
  
`[ @proxy_name = ] 'proxy_name'` Nom du compte proxy. *proxy_name* est de **type sysname** avec NULL comme valeur par défaut. S’il est spécifié, *proxy_id* doit avoir la valeur null. Pour obtenir *proxy_name*, interrogez la table système sysproxies.  
  
`[ @schedule_uid = ] 'schedule_uid'` GUID qui pointe vers une planification. *schedule_uid* est de type **uniqueidentifier** avec NULL comme valeur par défaut. S’il est spécifié, *schedule_name* doit avoir la valeur null. Pour obtenir *schedule_uid*, interrogez la table système sysschedules.  
  
 Lorsque *collection_mode* a la valeur 0, *schedule_uid* ou *schedule_name* doit être spécifié. Lorsque *collection_mode* a la valeur 1, *schedule_uid* ou *schedule_name* est ignoré s’il est spécifié.  
  
`[ @schedule_name = ] 'schedule_name'` Nom de la planification. *schedule_name* est de **type sysname** avec NULL comme valeur par défaut. S’il est spécifié, *schedule_uid* doit avoir la valeur null. Pour obtenir *schedule_name*, interrogez la table système sysschedules.  
  
`[ @logging_level = ] logging_level` Est le niveau de journalisation. *LOGGING_LEVEL* est de type **smallint** avec l’une des valeurs suivantes :  
  
 0 - informations de l'exécution du journal et événements [!INCLUDE[ssIS](../../includes/ssis-md.md)] qui effectuent le suivi des éléments suivants :  
  
-   Démarrage/arrêt de jeux d'éléments de collecte  
  
-   Démarrage/arrêt de packages  
  
-   Informations sur l'erreur  
  
 1 - Enregistrement de niveau 0 et :  
  
-   Statistiques d'exécution  
  
-   Progression de la collecte continuellement en cours d'exécution  
  
-   Événements d'avertissements de [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
 2 - Enregistrement de niveau 1 et informations d'événement détaillées de [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
 La valeur par défaut de *LOGGING_LEVEL* est 1.  
  
`[ @description = ] 'description'` Description du jeu d’entités de collecte. *Description* est de type **nvarchar (4000)** avec NULL comme valeur par défaut.  
  
`[ @collection_set_id = ] collection_set_id` Identificateur local unique pour le jeu d’Collections. *collection_set_id* est de **type int** avec output et est obligatoire.  
  
`[ @collection_set_uid = ] 'collection_set_uid'` GUID du jeu d’entités de collecte. *collection_set_uid* est de type **uniqueidentifier** avec output avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_syscollector_create_collection_set doit être exécuté dans le contexte de la base de données système msdb.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-collection-set-by-using-default-values"></a>R. Création d'un jeu d'éléments de collecte à l'aide de valeurs par défaut  
 L'exemple suivant crée un jeu d'éléments de collecte en spécifiant uniquement les paramètres requis. `@collection_mode` n'est pas nécessaire, mais le mode de collecte par défaut (mis en cache) nécessite de spécifier un ID de planification ou un nom de planification.  
  
```  
USE msdb;  
GO  
DECLARE @collection_set_id int;  
EXECUTE dbo.sp_syscollector_create_collection_set  
    @name = N'Simple collection set test 1',  
    @description = N'This is a test collection set that runs in non-cached mode.',  
    @collection_mode = 1,  
    @collection_set_id = @collection_set_id OUTPUT;  
GO  
```  
  
### <a name="b-creating-a-collection-set-by-using-specified-values"></a>B. Création d'un jeu d'éléments de collecte à l'aide de valeurs spécifiées  
 L'exemple suivant crée un jeu d'éléments de collecte en spécifiant des valeurs pour un grand nombre des paramètres.  
  
```  
USE msdb;  
GO  
DECLARE @collection_set_id int;  
DECLARE @collection_set_uid uniqueidentifier;  
SET @collection_set_uid = NEWID();  
EXEC dbo.sp_syscollector_create_collection_set  
    @name = N'Simple collection set test 2',  
    @collection_mode = 0,  
    @days_until_expiration = 365,  
    @description = N'This is a test collection set that runs in cached mode.',  
    @logging_level = 2,  
    @schedule_name = N'CollectorSchedule_Every_30min',  
    @collection_set_id = @collection_set_id OUTPUT,  
    @collection_set_uid = @collection_set_uid OUTPUT;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [Créer un jeu d’éléments de collecte personnalisé qui utilise le type de collecteur Requête T-SQL générique &#40;Transact-SQL&#41;](../../relational-databases/data-collection/create-custom-collection-set-generic-t-sql-query-collector-type.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [syscollector_collection_sets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)  
  
  
