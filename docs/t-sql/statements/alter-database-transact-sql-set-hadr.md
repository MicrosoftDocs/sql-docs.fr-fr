---
description: ALTER DATABASE SET HADR (Transact-SQL)
title: ALTER DATABASE SET HADR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET HADR
- SET_HADR_TSQL
- HADR_TSQL
- HADR
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE statement, AlwaysOn Availability Group
- ALTER DATABASE statement, SET HADR options
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
- Availability Groups [SQL Server], databases
ms.assetid: 20e6e803-d6d5-48d5-b626-d1e0a73d174c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7d62a25ccfdf4abb6231f226e7a846418680a155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184525"
---
# <a name="alter-database-transact-sql-set-hadr"></a>ALTER DATABASE (Transact-SQL) SET HADR 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique contient la syntaxe ALTER DATABASE pour la définition des options [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] sur une base de données secondaire. Une seule option SET HADR est autorisée par instruction ALTER DATABASE. Ces options sont prises en charge uniquement sur les réplicas secondaires.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER DATABASE database_name  
   SET HADR   
   {  
        { AVAILABILITY GROUP = group_name | OFF }  
   | { SUSPEND | RESUME }  
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *database_name*  
 Nom de la base de données secondaire à modifier.  
  
 SET HADR  
 Exécute la commande [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] spécifiée sur le serveur spécifié.  
  
 { AVAILABILITY GROUP **=** _group_name_ | OFF }  
 Joint ou supprime la base de données de disponibilité dans le groupe de disponibilité spécifié, comme suit :  
  
 *group_name*  
 Attache la base de données spécifiée sur le réplica secondaire hébergé par l'instance de serveur sur laquelle vous exécutez la commande sur le groupe de disponibilité spécifié par group_name.  
  
 Les conditions préalables requises à cette opération sont les suivantes :  
  
-   La base de données doit déjà avoir été ajoutée au groupe de disponibilité sur le réplica principal.  
  
-   Le réplica principal doit être actif. Pour plus d’informations sur la façon de dépanner un réplica principal inactif, consultez [Résolution des problèmes de configuration des groupes de disponibilité AlwaysOn (SQL Server)](/previous-versions/sql/sql-server-2012/ff878308(v=sql.110)).  
  
-   Le réplica principal doit être en ligne et le réplica secondaire doit être connecté au réplica principal.  
  
-   La base de données secondaire doit avoir été restaurée à l'aide de WITH NORECOVERY à partir de sauvegardes de base de données et de fichiers journaux récente de la base de données primaire, avec une dernière sauvegarde de fichier journal suffisamment récente pour permettre à la base de données secondaire de rattraper la base de données primaire.  
  
    > [!NOTE]  
    >  Pour ajouter une base de données au groupe de disponibilité, connectez-vous à l’instance de serveur qui héberge le réplica principal, puis utilisez l’instruction [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)*group_name* ADD DATABASE *database_name*.  
  
 Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 OFF  
 Supprime la base de données secondaire spécifiée du groupe de disponibilité spécifié.  
  
 La suppression d'une base de données secondaire peut être utile si elle est trop éloignée de la base de données primaire et que vous ne souhaitez pas attendre que la base de données secondaire rattrape son retard. Après avoir supprimé la base de données secondaire, vous pouvez la mettre à jour en restaurant une séquence de sauvegardes se terminant par une sauvegarde récente des fichiers journaux (à l’aide de RESTORE... WITH NORECOVERY).  
  
> [!IMPORTANT]  
>  Pour supprimer complètement une base de données d’un groupe de disponibilité, connectez-vous à l’instance de serveur qui héberge le réplica principal, puis utilisez l’instruction [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)*group_name* REMOVE DATABASE *availability_database_name*. Pour plus d’informations, consultez [Supprimer une base de données primaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
 SUSPEND  
 Suspend le déplacement des données sur une base de données secondaire. Une commande SUSPEND retourne dès qu'elle est acceptée par le réplica qui héberge la base de données cible, mais en réalité, l'interruption de la base de données se produit de façon asynchrone.  
  
 L'étendue de l'impact dépend de l'emplacement où vous exécutez l'instruction ALTER DATABASE :  
  
-   Si vous suspendez une base de données secondaire sur un réplica secondaire, seule la base de données secondaire locale est suspendue. Les connexions existantes sur le réplica secondaire accessible en lecture restent utilisables. Les nouvelles connexions à la base de données suspendue sur le réplica secondaire accessible en lecture ne sont pas autorisées tant que le déplacement des données n'a pas repris.  
  
-   Si vous suspendez une base de données sur le réplica principal, le déplacement des données est suspendu vers les bases de données secondaires correspondantes sur chaque réplica secondaire. Les connexions existantes sur un réplica secondaire accessible en lecture restent utilisables, et les nouvelles connexions de tentative de lecture ne se connecteront pas aux réplicas secondaires accessibles en lecture.  
  
-   Lorsque le déplacement des données est suspendu en raison d'un basculement manuel forcé, les connexions au nouveau réplica secondaire ne sont pas autorisées pendant que le déplacement des données est suspendu.  
  
 Lorsqu'une base de données sur un réplica secondaire est suspendue, la base de données et le réplica deviennent désynchronisés et sont marquées comme NOT SYNCHRONIZED.  
  
> [!IMPORTANT]  
>  Lorsqu'une base de données secondaire est suspendue, la file d'attente d'envoi de la base de données primaire correspondante accumule des enregistrements du journal des transactions non envoyés. Les connexions au réplica secondaire retournent des données qui étaient disponibles lorsque le déplacement des données a été suspendu.  
  
> [!NOTE]  
>  L’interruption et la reprise d’une base de données secondaire Always On n’affectent pas directement la disponibilité de la base de données primaire, bien que l’interruption d’une base de données secondaire puisse avoir une incidence sur les capacités de redondance et de basculement de la base de données primaire, tant que la base de données secondaire interrompue n’a pas repris. Ce comportement diffère de la mise en miroir de bases de données dans laquelle l'état de mise en miroir est interrompu à la fois sur la base de données miroir et la base de données principale tant que la mise en miroir n'a pas repris. L’interruption d’une base de données principale Always On interrompt le déplacement des données sur toutes les bases de données secondaires correspondantes, et les capacités de redondance et de basculement cessent pour cette base de données tant que la base de données principale n’a pas repris.  
  
 Pour plus d’informations, consultez [Suspendre une base de données de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md).  
  
 RESUME  
 Reprend le déplacement des données suspendu sur la base de données secondaire spécifiée. Une commande RESUME retourne dès qu'elle est acceptée par le réplica qui héberge la base de données cible, mais en réalité, la reprise de la base de données se produit de façon asynchrone.  
  
 L'étendue de l'impact dépend de l'emplacement où vous exécutez l'instruction ALTER DATABASE :  
  
-   Si vous reprenez une base de données secondaire sur un réplica secondaire, seule la base de données secondaire locale est reprise. Le déplacement des données est repris, à moins que la base de données ait également été suspendue sur le réplica principal.  
  
-   Si vous reprenez une base de données sur le réplica principal, le déplacement des données est repris vers chaque réplica secondaire sur lequel la base de données secondaire correspondante n'a pas été suspendue localement. Pour reprendre une base de données secondaire qui a été individuellement suspendue sur un réplica secondaire, connectez-vous à l'instance de serveur qui héberge le réplica secondaire et reprenez la base de données à cet endroit.  
  
     En mode de validation synchrone, l'état de la base de données devient SYNCHRONIZING. Si aucune autre base de données n'est actuellement suspendue, l'état du réplica devient également SYNCHRONIZING.  
  
     Pour plus d'informations, consultez [Reprendre une base de données de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md).  
  
## <a name="database-states"></a>États d'une base de données  
 Lorsqu'une base de données secondaire est attachée à un groupe de disponibilité, le réplica secondaire local modifie l'état de cette base de données secondaire de RESTORING à ONLINE. Si une base de données secondaire est supprimée du groupe de disponibilité, le réplica secondaire local la fait repasser à l'état RESTORING. Cela vous permet d'appliquer des sauvegardes de journaux ultérieures depuis la base de données primaire vers cette base de données secondaire.  
  
## <a name="restrictions"></a>Restrictions  
 Exécutez les instructions ALTER DATABASE en dehors des transactions et des traitements.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER sur la base de données. La jointure d’une base de données à un groupe de disponibilité nécessite l’appartenance au rôle de base de données fixe **db_owner**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant joint la base de données secondaire, `AccountsDb1`, au réplica secondaire local du groupe de disponibilité `AccountsAG`.  
  
```sql  
ALTER DATABASE AccountsDb1 SET HADR AVAILABILITY GROUP = AccountsAG;  
```  
  
> [!NOTE]  
>  Pour consulter cette instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisée en contexte, consultez [Créer un groupe de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [Vue d’ensemble des groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) [Résoudre des problèmes de configuration des groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md) 
  
