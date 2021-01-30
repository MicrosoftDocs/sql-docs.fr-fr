---
description: sp_help_fulltext_catalogs (Transact-SQL)
title: sp_help_fulltext_catalogs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_catalogs_TSQL
- sp_help_fulltext_catalogs
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_catalogs
ms.assetid: 1b94f280-e095-423f-88bc-988c9349d44c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 12382d003a5f1207506164c35b171a9c1a5e5385
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198328"
---
# <a name="sp_help_fulltext_catalogs-transact-sql"></a>sp_help_fulltext_catalogs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne l'identificateur, le nom, le répertoire racine, l'état et le nombre de tables indexées en texte intégral du catalogue de texte intégral spécifié.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt l’affichage catalogue [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_fulltext_catalogs [ @fulltext_catalog_name = ] 'fulltext_catalog_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` Nom du catalogue de texte intégral. *fulltext_catalog_name* est de **type sysname**. Si ce paramètre est omis ou s'il a la valeur NULL, des informations sur tous les catalogues de texte intégral associés à la base de données active sont retournées.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Cette table montre le jeu de résultats, classé selon le paramètre **ftcatid**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**fulltext_catalog_id**|**smallint**|Identificateur du catalogue de texte intégral.|  
|**NAME**|**sysname**|Nom du catalogue de texte intégral.|  
|**PATH**|**nvarchar(260)**|À partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], cette clause n'a aucun effet.|  
|**ÉTAT**|**int**|État du remplissage de l'index de recherche en texte intégral du catalogue :<br /><br /> 0 = Inactif <br /><br /> 1 = Remplissage complet en cours<br /><br /> 2 = En pause <br /><br /> 3 = Accéléré<br /><br /> 4 = Récupération<br /><br /> 5 = Arrêt<br /><br /> 6 = Remplissage incrémentiel en cours<br /><br /> 7 = Indexation en cours<br /><br /> 8 = Disque plein Suspendu<br /><br /> 9 = Suivi des modifications<br /><br /> NULL = L'utilisateur n'a pas l'autorisation VIEW sur le catalogue de texte intégral, la base de données n'est pas activée en texte intégral ou le composant de texte intégral n'est pas installé.|  
|**NUMBER_FULLTEXT_TABLES**|**int**|Nombre de tables indexées en texte intégral associées au catalogue.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution reviennent par défaut aux membres du rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne des informations sur le catalogue de texte intégral `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help_fulltext_catalogs 'Cat_Desc' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [FULLTEXTCATALOGPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [sp_fulltext_catalog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)   
 [sp_help_fulltext_catalogs_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
