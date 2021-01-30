---
description: sp_settriggerorder (Transact-SQL)
title: sp_settriggerorder (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_settriggerorder
- sp_settriggerorder_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_settriggerorder
ms.assetid: 8b75c906-7315-486c-bc59-293ef12078e8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f7977873f4b2887b736ffc16f5592340f5b87dee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207370"
---
# <a name="sp_settriggerorder-transact-sql"></a>sp_settriggerorder (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Spécifie les déclencheurs AFTER activés en premier ou en dernier. L'ordre d'exécution des déclencheurs AFTER activés entre les premier et dernier déclencheurs est indéfini.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_settriggerorder [ @triggername = ] '[ triggerschema. ] triggername'   
    , [ @order = ] 'value'   
    , [ @stmttype = ] 'statement_type'   
    [ , [ @namespace = ] { 'DATABASE' | 'SERVER' | NULL } ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @triggername = ] '[ _triggerschema.] _triggername'` Nom du déclencheur et du schéma auquel il appartient, le cas échéant, dont l’ordre doit être défini ou modifié. [_triggerschema_**.**] *triggername* est de **type sysname**. Si le nom ne correspond pas à un déclencheur ou si le nom correspond à un déclencheur INSTEAD OF, la procédure retourne une erreur. *triggerschema* ne peut pas être spécifié pour les déclencheurs DDL ou Logon.  
  
`[ @order = ] 'value'` Paramètre de la nouvelle commande du déclencheur. la *valeur* est de type **varchar (10)** et peut prendre l’une des valeurs suivantes.  
  
> [!IMPORTANT]  
>  Le **premier** et le **dernier** déclencheur doivent être deux déclencheurs différents.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Première**|Le déclencheur est activé en premier.|  
|**Dernière**|Le déclencheur est activé en dernier.|  
|**Aucun**|L'ordre d'activation du déclencheur n'est pas défini.|  
  
`[ @stmttype = ] 'statement_type'` Spécifie l’instruction SQL qui déclenche le déclencheur. *statement_type* est de type **varchar (50)** et peut être insertion, mise à jour, suppression, ouverture de session ou n’importe quel [!INCLUDE[tsql](../../includes/tsql-md.md)] événement d’instruction listé dans des [événements DDL](../../relational-databases/triggers/ddl-events.md). Les groupes d'événements ne peuvent pas être spécifiés.  
  
 Un déclencheur peut être désigné comme **premier** ou **dernier** déclencheur pour un type d’instruction uniquement après que ce déclencheur a été défini en tant que déclencheur pour ce type d’instruction. Par exemple, le déclencheur **TR1** peut être désigné en **premier** pour l’insertion sur la table **T1** si **TR1** est défini en tant que déclencheur INSERT. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne une erreur si **TR1**, qui a été défini uniquement comme déclencheur d’insertion, est défini en tant que déclencheur **First** ou **Last** pour une instruction Update. Pour plus d'informations, consultez la section Notes.  
  
 **\@ espace de noms =** { **'base de données**'  |  **'serveur'** | NUL  
 Quand *triggername* est un déclencheur DDL, l' **\@ espace de noms** spécifie si *triggername* a été créé avec une étendue de base de données ou de serveur. Si *triggername* est un déclencheur LOGON, le serveur doit être spécifié. Pour plus d’informations sur la portée du déclencheur DDL, consultez [déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md). S’il n’est pas spécifié, ou si NULL est spécifié, *triggername* est un déclencheur DML.  
  
* Le serveur s’applique à : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) et 1 (échec)  
  
## <a name="remarks"></a>Notes  
  
## <a name="dml-triggers"></a>Déclencheurs DML  
 Il ne peut y avoir qu’un seul **premier** et **dernier** déclencheur pour chaque instruction sur une table unique.  
  
 Si un **premier** déclencheur est déjà défini sur la table, la base de données ou le serveur, vous ne pouvez pas désigner un nouveau déclencheur comme **tout d’abord** pour la même table, base de données ou serveur pour le même *statement_type*. Cette restriction applique également les **derniers** déclencheurs.  
  
 La réplication génère automatiquement un premier déclencheur pour toute table qui est incluse dans un abonnement avec mise à jour immédiate ou en attente. Elle nécessite un déclencheur qui soit le premier. Elle génère une erreur si vous essayez d'inclure une table détenant un premier déclencheur dans un abonnement mis à jour immédiatement ou en attente. Si vous tentez de définir un déclencheur comme premier déclencheur après qu’une table a été incluse dans un abonnement, **sp_settriggerorder** retourne une erreur. Si vous utilisez ALTER TRIGGER sur le déclencheur de réplication ou si vous utilisez **sp_settriggerorder** pour remplacer le déclencheur de réplication par un déclencheur **Last** ou **None** , l’abonnement ne fonctionne pas correctement.  
  
## <a name="ddl-triggers"></a>Déclencheurs DDL  
 Si un déclencheur DDL avec une étendue de base de données et un déclencheur DDL avec une étendue de serveur existent sur le même événement, vous pouvez spécifier que les deux déclencheurs sont un **premier** déclencheur ou un **dernier** déclencheur. Toutefois, les déclencheurs avec étendue de serveur se déclenchent toujours en premier. En général, l'ordre d'exécution des déclencheurs DDL qui existent sur le même événement est le suivant :  
  
1.  Le déclencheur de niveau serveur est marqué en **premier**.  
  
2.  Autres déclencheurs de niveau serveur.  
  
3.  Déclencheur de niveau serveur marqué en **dernier**.  
  
4.  Le déclencheur de niveau base de données est marqué en **premier**.  
  
5.  Autres déclencheurs de niveau base de données.  
  
6.  Déclencheur de niveau base de données marqué en **dernier**.  
  
## <a name="general-trigger-considerations"></a>Considérations générales sur les déclencheurs  
 Si une instruction ALTER TRIGGER modifie un premier ou un dernier déclencheur, le **premier** ou le **dernier** attribut défini à l’origine sur le déclencheur est supprimé et la valeur est remplacée par **None**. La valeur de la commande doit être réinitialisée à l’aide de **sp_settriggerorder**.  
  
 Si le même déclencheur doit être désigné en tant que premier ou dernier ordre pour plusieurs types d’instructions, **sp_settriggerorder** doit être exécutée pour chaque type d’instruction. En outre, le déclencheur doit d’abord être défini pour un type d’instruction avant de pouvoir être désigné comme **premier** ou **dernier** déclencheur à déclencher pour ce type d’instruction.  
  
## <a name="permissions"></a>Autorisations  
 Pour définir l'ordre d'un déclencheur DDL avec étendue de serveur (à l'aide de la clause ON ALL SERVER) ou d'un déclencheur de connexion, requiert l'autorisation CONTROL SERVER sur le serveur.  
  
 Pour définir l'ordre d'un déclencheur DDL avec étendue de base de données (à l'aide de la clause ON DATABASE), requiert l'autorisation ALTER ANY DATABASE DDL TRIGGER.  
  
 Pour définir l'ordre d'un déclencheur DML, requiert l'autorisation ALTER sur la table ou vue sur laquelle le déclencheur est configuré.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-the-firing-order-for-a-dml-trigger"></a>R. Définition de l'ordre d'activation d'un déclencheur DML  
 L'exemple suivant spécifie que le déclencheur `uSalesOrderHeader` est le premier déclencheur à activer après une opération `UPDATE` sur la table `Sales.SalesOrderHeader`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'Sales.uSalesOrderHeader', @order='First', @stmttype = 'UPDATE';  
```  
  
### <a name="b-setting-the-firing-order-for-a-ddl-trigger"></a>B. Définition de l'ordre d'activation d'un déclencheur DDL  
 L'exemple suivant spécifie que le déclencheur `ddlDatabaseTriggerLog` est le premier déclencheur à activer après un événement `ALTER_TABLE` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'ddlDatabaseTriggerLog', @order='First', @stmttype = 'ALTER_TABLE', @namespace = 'DATABASE';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
  
