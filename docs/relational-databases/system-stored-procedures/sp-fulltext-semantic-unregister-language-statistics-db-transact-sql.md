---
description: sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL)
title: sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_semantic_unregister_language_statistics_db_TSQL
- sp_fulltext_semantic_unregister_language_statistics_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_semantic_unregister_language_statistics_db
ms.assetid: 1426ca4a-9a76-489e-98da-8f6d13ff9732
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2d81bc4f43e93cb58b425224fb39f31a27fe7f90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189559"
---
# <a name="sp_fulltext_semantic_unregister_language_statistics_db-transact-sql"></a>sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Annule l'inscription d'une base de données de statistiques linguistiques de sémantique existante de l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et supprime toutes les métadonnées associées.  
  
 Cette instruction ne détache pas la base de données ni ne supprime le fichier de base de données physique du système de fichiers. Après avoir annulé l'inscription de la base de données, vous pouvez la détacher et supprimer le fichier de base de données physique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
EXEC sp_fulltext_semantic_unregister_language_statistics_db;  
GO  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 Cette procédure ne requiert pas d'arguments. Étant donné qu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne dispose que d'une base de données de statistiques linguistiques de sémantique, il n'est pas nécessaire d'identifier la base de données.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-set"></a>Jeu de résultats  
 Aucun.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Lorsque l'inscription d'une base de données de statistiques linguistiques de sémantique est annulée, toutes les métadonnées associées sont également supprimées.  
  
 **sp_fulltext_semantic_unregister_language_statistics_db** effectue les étapes suivantes :  
  
1.  Vérifie qu'aucun remplissage sémantique n'est en cours pour l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Supprime toutes les métadonnées associées à la base de données de statistiques linguistiques de sémantique spécifiée.  

 Pour plus d’informations, consultez [Installer et configurer la recherche sémantique](../../relational-databases/search/install-and-configure-semantic-search.md).  
  
## <a name="metadata"></a>Métadonnées  
 Pour plus d’informations sur la base de données Base de langages statistiques pour la recherche sémantique installée sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , interrogez l’affichage catalogue [sys.fulltext_semantic_language_statistics_database &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment annuler l’inscription de la base de données Base de langages statistiques pour la recherche sémantique en appelant **sp_fulltext_semantic_unregister_language_statistics_db**.  
  
```sql  
EXEC sp_fulltext_semantic_unregister_language_statistics_db;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Installer et configurer la recherche sémantique](../../relational-databases/search/install-and-configure-semantic-search.md)  
  
  
