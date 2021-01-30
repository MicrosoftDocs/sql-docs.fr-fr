---
description: sp_clean_db_file_free_space (Transact-SQL)
title: sp_clean_db_file_free_space (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_clean_db_file_free_space
- sp_clean_db_file_free_space_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ghost records
- sp_clean_db_file_free_space
ms.assetid: 3eb53a67-969d-4cb8-9681-b1c8e6fd55b6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 032cc0a56b9ae181a4c3f3206bcc95a63485d45c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206537"
---
# <a name="sp_clean_db_file_free_space-transact-sql"></a>sp_clean_db_file_free_space (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime des informations qui sont restées sur les pages de la base de données en raison des routines de modification de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sp_clean_db_file_free_space nettoie toutes les pages d’un seul fichier d’une base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
sp_clean_db_file_free_space   
  [ @dbname = ] 'database_name'   
  , [ @fileid = ] 'file_number'   
  [ , [ @cleaning_delay = ] 'delay_in_seconds' ] [;]  
```  
  
## <a name="arguments"></a>Arguments  
 @dbname = '*database_name*'  
 Nom de la base de données à nettoyer. *dbname* est de **type sysname** et ne peut pas avoir la valeur null.  
  
 @fileid = '*file_number*'  
 ID du fichier de données à nettoyer. *file_number* est de **type int** et ne peut pas avoir la valeur null.  
  
 @cleaning_delay = '*delay_in_seconds*'  
 Spécifie un intervalle entre le nettoyage des pages. Cela réduit l'effet sur le système d'E/S. *delay_in_seconds* est de **type int** avec 0 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Les opérations de suppression d'une table ou les opérations de mise à jour qui entraînent le déplacement d'une ligne peuvent libérer immédiatement de l'espace sur une page en supprimant les références à cette ligne. Cependant, dans certains cas, la ligne peut physiquement être conservée sur la page de données sous la forme d'un enregistrement fantôme. Les enregistrements fantômes sont supprimés régulièrement par un processus en arrière-plan. Ces données résiduelles ne sont pas retournées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] en réponse aux requêtes. Toutefois, dans les environnements dans lesquels la sécurité physique des données ou des fichiers de sauvegarde est menacée, vous pouvez utiliser `sp_clean_db_file_free_space` pour nettoyer ces enregistrements fantômes. Pour effectuer cette opération pour tous les fichiers de base de données à la fois, utilisez [sp_clean_db_free_space (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-clean-db-free-space-transact-sql.md). 
  
 La durée d'exécution de sp_clean_db_file_free_space dépend de la taille du fichier, de l'espace disponible et de la capacité du disque. Étant donné que l’exécution `sp_clean_db_file_free_space` de peut affecter de manière significative l’activité d’e/s, nous vous recommandons d’exécuter cette procédure en dehors des heures de fonctionnement habituelles.  
  
 Avant d’exécuter `sp_clean_db_file_free_space` , nous vous recommandons de créer une sauvegarde complète de base de données.  
  
 La procédure stockée [sp_clean_db_free_space](../../relational-databases/system-stored-procedures/sp-clean-db-free-space-transact-sql.md) associée nettoie tous les fichiers de la base de données.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au `db_owner` rôle de base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant nettoie toutes les informations résiduelles du fichier de données principal de la base de données `AdventureWorks2012`.  
  
```sql  
USE master;  
GO  
EXEC sp_clean_db_file_free_space @dbname = N'AdventureWorks2012', @fileid = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Guide de processus de nettoyage fantôme](../ghost-record-cleanup-process-guide.md)    
 [sp_clean_db_free_space (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-clean-db-free-space-transact-sql.md)
   
