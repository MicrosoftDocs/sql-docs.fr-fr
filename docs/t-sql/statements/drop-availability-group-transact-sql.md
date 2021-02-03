---
description: DROP AVAILABILITY GROUP (Transact-SQL)
title: DROP AVAILABILITY GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_AVAILABILITY_GROUP_TSQL
- DROP AVAILABILITY GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], Transact-SQL statements
- DROP AVAILABILITY GROUP statement
- Availability Groups [SQL Server], dropping
ms.assetid: c1600289-c990-454a-b279-dba0ebd5d63e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: d50eae15352e3de39b17c9ea57b5422bf41b5438
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191007"
---
# <a name="drop-availability-group-transact-sql"></a>DROP AVAILABILITY GROUP (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime le groupe de disponibilité spécifié et tous ses réplicas. Si une instance de serveur qui héberge l'un des réplicas de disponibilité est hors connexion lorsque vous supprimez un groupe de disponibilité, une fois de nouveau en ligne, l'instance de serveur supprimera le réplica de disponibilité local. La suppression d'un groupe de disponibilité supprime également l'écouteur du groupe de disponibilité associé, le cas échéant.  
  
> [!IMPORTANT]  
>  si cela est possible, supprimez le groupe de disponibilité uniquement lorsque vous êtes connecté à l'instance de serveur qui héberge le réplica principal. Si le groupe de disponibilité est supprimé du réplica principal, les modifications sont autorisées dans les bases de données primaires précédentes (sans protection haute disponibilité). Quand vous supprimez un groupe de disponibilité d’un réplica secondaire, le réplica principal reste à l’état **RESTORING** et les modifications au niveau des bases de données ne sont pas autorisées.  
  
 Pour plus d’informations sur les autres façons de supprimer un groupe de disponibilité, consultez [Supprimer un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP AVAILABILITY GROUP group_name   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *group_name*  
 Spécifie le nom du groupe de disponibilité à supprimer.  
  
## <a name="limitations-and-recommendations"></a>Recommandations et limitations  
  
-   L’exécution de **DROP AVAILABILITY GROUP** exige que la fonctionnalité Groupes de disponibilité AlwaysOn soit activée sur l’instance de serveur. Pour plus d’informations, consultez [Activer et désactiver les groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
-   **DROP AVAILABILITY GROUP** ne peut pas être exécuté dans des traitements ou des transactions. En outre, les expressions et les variables ne sont pas prises en charge.  
  
-   Vous pouvez supprimer un groupe de disponibilité de tout nœud de clustering de basculement Windows Server (WSFC) qui possède les informations d'identification de sécurité correctes pour le groupe de disponibilité. Cela vous permet de supprimer un groupe de disponibilité lorsqu'il ne reste aucun de ses réplicas de disponibilité.  
  
    > [!IMPORTANT]  
    >  Évitez de supprimer un groupe de disponibilité lorsque le cluster de clustering de basculement Windows Server (WSFC) n'a aucun quorum. Si vous devez supprimer un groupe de disponibilité lorsque le cluster ne dispose pas de quorum, les métadonnées du groupe de disponibilité stockées dans le cluster nesont pas supprimées. Après que le cluster a regagné le quorum, vous devez supprimer à nouveau le groupe de disponibilité pour le supprimer du cluster WSFC.  
  
-   Sur un réplica secondaire, **DROP AVAILABILITY GROUP** ne doit être utilisé qu’en cas d’urgence. Cela est dû au fait que la suppression d'un groupe de disponibilité met le groupe de disponibilité hors connexion. Si vous supprimez le groupe de disponibilité d’un réplica secondaire, le réplica principal ne peut pas déterminer si l’état **OFFLINE** était la conséquence d’une perte de quorum, d’un basculement forcé ou d’une commande **DROP AVAILABILITY GROUP**. Le réplica principal passe à l’état **RESTORING** pour éviter un fractionnement possible des partitions. Pour plus d’informations, consultez [Fonctionnement : comportements de DROP AVAILABILITY GROUP](/archive/blogs/psssql/how-it-works-drop-availability-group-behaviors) (blog des ingénieurs du Service clientèle et du Support technique de SQL Server).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite une autorisation **ALTER AVAILABILITY GROUP** sur le groupe de disponibilité, une autorisation **CONTROL AVAILABILITY GROUP**, une autorisation **ALTER ANY AVAILABILITY GROUP** ou une autorisation **CONTROL SERVER**. Pour supprimer un groupe de disponibilité qui n’est pas hébergé par l’instance de serveur local, vous avez besoin d’une autorisation **CONTROL SERVER** ou **CONTROL** sur ce groupe de disponibilité.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant supprime le groupe de disponibilité `AccountsAG`.  
  
```sql  
DROP AVAILABILITY GROUP AccountsAG;  
```  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Fonctionnement : comportements de DROP AVAILABILITY GROUP](/archive/blogs/psssql/how-it-works-drop-availability-group-behaviors) (blog des ingénieurs du Service clientèle et du Support technique de SQL Server)  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [Supprimer un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
