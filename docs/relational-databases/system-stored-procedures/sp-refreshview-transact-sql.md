---
description: sp_refreshview (Transact-SQL)
title: sp_refreshview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refreshview
- sp_refreshview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refreshview
ms.assetid: 9ce1d07c-ee66-4a83-8c73-cd2cc104dd08
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da179448924381de2dddbf5613194c30df1f01bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201270"
---
# <a name="sp_refreshview-transact-sql"></a>sp_refreshview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour les métadonnées de la vue non liée au schéma spécifiée. Les métadonnées persistantes d'une vue peuvent devenir obsolètes en raison des modifications des objets sous-jacents dont dépend l'affichage.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_refreshview [ @viewname = ] 'viewname'   
```  
  
## <a name="arguments"></a>Arguments  
`[ @viewname = ] 'viewname'` Nom de la vue. *viewName* est de type **nvarchar**, sans valeur par défaut. *viewName* peut être un identificateur à parties multiples, mais ne peut faire référence qu’à des vues de la base de données active.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou un nombre différent de zéro (échec)  
  
## <a name="remarks"></a>Notes  
 Si une vue n’est pas créée avec SCHEMABINDING, **sp_refreshview** doit être exécutée lorsque des modifications sont apportées aux objets sous-jacents de la vue qui affectent la définition de la vue. Autrement, la vue risque de produire des résultats imprévisibles en cas d'interrogation.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER sur la vue ainsi que l'autorisation REFERENCES sur les types CLR (Common Language Runtime) définis par l'utilisateur et sur les collections de schémas XML référencés par les colonnes de la vue.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-updating-the-metadata-of-a-view"></a>R. Mise à jour des métadonnées d'une vue  
 L'exemple suivant actualise les métadonnées de la vue `Sales.vIndividualCustomer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sp_refreshview N'Sales.vIndividualCustomer';  
```  
  
### <a name="b-creating-a-script-that-updates-all-views-that-have-dependencies-on-a-changed-object"></a>B. Création d'un script qui met à jour toutes les vues ayant des dépendances envers un objet modifié  
 Supposez que la table `Person.Person` a été modifiée d'une manière qui affecte la définition de toute vue créée dessus. L'exemple suivant crée un script qui actualise les métadonnées de toutes les vues qui ont une dépendance envers une table `Person.Person`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT 'EXEC sp_refreshview ''' + name + ''''   
FROM sys.objects AS so   
INNER JOIN sys.sql_expression_dependencies AS sed   
    ON so.object_id = sed.referencing_id   
WHERE so.type = 'V' AND sed.referenced_id = OBJECT_ID('Person.Person');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_refreshsqlmodule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md)  
  
  
