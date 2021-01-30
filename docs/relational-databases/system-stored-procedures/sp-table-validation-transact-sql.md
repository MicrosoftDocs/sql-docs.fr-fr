---
description: sp_table_validation (Transact-SQL)
title: sp_table_validation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_table_validation_TSQL
- sp_table_validation
helpviewer_keywords:
- sp_table_validation
ms.assetid: 31b25f9b-9b62-496e-a97e-441d5fd6e767
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 255c30ea6b3144e781007bff3ae748c8e5a960ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189255"
---
# <a name="sp_table_validation-transact-sql"></a>sp_table_validation (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur le nombre de lignes ou sur la somme de contrôle d'une table ou d'une vue indexée, ou bien compare ces informations avec la table ou la vue indexée spécifiée. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication et sur la base de données d'abonnement de l'Abonné. *Non pris en charge pour les serveurs de publication Oracle*.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_table_validation [ @table = ] 'table'  
    [ , [ @expected_rowcount = ] type_of_check_requested OUTPUT]  
    [ , [ @expected_checksum = ] expected_checksum OUTPUT]  
    [ , [ @rowcount_only = ] rowcount_only ]  
    [ , [ @owner = ] 'owner' ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @table_name = ] table_name ]  
    [ , [ @column_list = ] 'column_list' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table = ] 'table'` Nom de la table. *table* est de **type sysname**, sans valeur par défaut.  
  
`[ @expected_rowcount = ] expected_rowcountOUTPUT` Spécifie s’il faut retourner le nombre attendu de lignes dans la table. *expected_rowcount* est de **type int**, avec NULL comme valeur par défaut. Si la valeur est NULL, le nombre de lignes réel est retourné en tant que paramètre de sortie. Si une valeur est fournie, celle-ci est confrontée au nombre réel de lignes en vue d'une identification des éventuelles différences.  
  
`[ @expected_checksum = ] expected_checksumOUTPUT` Spécifie s’il faut retourner la somme de contrôle attendue pour la table. *expected_checksum* est de **type numeric**, avec NULL comme valeur par défaut. Si la valeur est NULL, la somme de contrôle réelle est retournée en tant que paramètre de sortie. Si une valeur est fournie, celle-ci est confrontée à la somme de contrôle réelle en vue d'une identification des éventuelles différences.  
  
`[ @rowcount_only = ] type_of_check_requested` Spécifie le type de somme de contrôle ou de RowCount à effectuer. *type_of_check_requested* est de type **smallint**, avec **1** comme valeur par défaut.  
  
 Si la **valeur est 0**, effectuez un comptage de lignes et une somme de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contrôle compatible 7,0.  
  
 Si **1**, effectuez une vérification du ROWCOUNT uniquement.  
  
 Si la condition est **2**, effectuez un comptage de lignes et une somme de contrôle binaire.  
  
`[ @owner = ] 'owner'` Nom du propriétaire de la table. *owner* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @full_or_fast = ] full_or_fast` Méthode utilisée pour calculer le RowCount. *full_or_fast* est de **type tinyint**, avec **2** comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Effectue un comptage total à l'aide de COUNT(*).|  
|**1**|Effectue un comptage rapide à partir de **sysindexes. Rows**. Le comptage des lignes dans **sysindexes** est beaucoup plus rapide que le comptage des lignes dans la table réelle. Toutefois, étant donné que **sysindexes** est mis à jour tardivement, le ROWCOUNT peut ne pas être précis.|  
|**2** (par défaut)|Exécute un décompte rapide conditionnel en essayant d'abord la méthode rapide. Si la méthode rapide affiche des différences, revient à la méthode totale. Si *expected_rowcount* a la valeur null et que la procédure stockée est utilisée pour obtenir la valeur, un nombre total (*) est toujours utilisé.|  
  
`[ @shutdown_agent = ] shutdown_agent` Si le Agent de distribution exécute **sp_table_validation**, spécifie si le agent de distribution doit s’arrêter immédiatement à la fin de la validation. *shutdown_agent* est de **bit**, avec **0** comme valeur par défaut. Si la **valeur est 0**, l’agent de réplication ne s’arrête pas. Si la valeur est **1**, l’erreur 20578 est déclenchée et l’agent de réplication est signalé pour s’arrêter. Ce paramètre est ignoré lorsque **sp_table_validation** est exécutée directement par un utilisateur.  
  
`[ @table_name = ] table_name` Nom de la table de la vue utilisée pour les messages de sortie. *table_name* est de **type sysname**, avec **\@ table** comme valeur par défaut.  
  
`[ @column_list = ] 'column_list'` Liste des colonnes à utiliser dans la fonction de somme de contrôle. *column_list* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut. Active la validation d'articles de fusion pour spécifier une liste de colonnes excluant les colonnes calculées et les colonnes timestamp.  
  
## <a name="return-code-values"></a>Codet de retour  
 Si vous effectuez une validation de la somme de contrôle et que la somme de contrôle attendue est égale à la somme de contrôle de la table, **sp_table_validation** retourne un message indiquant que la validation de la somme de contrôle a réussi. Sinon, elle retourne un message indiquant que la table peut ne plus être synchronisée et indique la différence entre le nombre de lignes attendu et le nombre réel.  
  
 Si vous effectuez une validation du nombre de lignes et que le nombre de lignes attendu est égal au nombre de la table, **sp_table_validation** retourne un message indiquant que la table a réussi la validation du nombre de lignes. Sinon, elle retourne un message indiquant que la table peut ne plus être synchronisée et indique la différence entre le nombre de lignes attendu et le nombre réel.  
  
## <a name="remarks"></a>Notes  
 **sp_table_validation** est utilisé dans tous les types de réplications. **sp_table_validation** n’est pas pris en charge pour les serveurs de publication Oracle.  
  
 La somme de contrôle calcule une vérification de redondance cyclique de 32 bits (CRC) sur l'image de la ligne entière de la page. La somme de contrôle ne vérifie pas les colonnes de manière sélective et ne peut pas s'exécuter sur une vue ou une partition verticale de la table. En outre, la somme de contrôle ignore le contenu des colonnes de **texte** et d' **image** (par conception).  
  
 Lors d'une somme de contrôle, la structure de la table doit être identique sur les deux serveurs ; les tables doivent posséder les mêmes colonnes, dans le même ordre, les mêmes types et longueurs de données et les mêmes conditions NULL/NOT NULL. Par exemple, si le serveur de publication a exécuté une instruction CREATE TABLE, puis une instruction ALTER TABLE pour ajouter des colonnes, mais que le script appliqué au niveau de l'Abonné est une simple CREATE TABLE, la structure n'est PAS la même. Si vous n’êtes pas certain que la structure des deux tables est identique, examinez [syscolumns](../../relational-databases/system-compatibility-views/sys-syscolumns-transact-sql.md) et vérifiez que le décalage de chaque table est identique.  
  
 Les valeurs à virgule flottante sont susceptibles de générer des différences de somme de contrôle si l' **utilitaire bcp** en mode caractère a été utilisé, ce qui est le cas si la publication a des abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cela est dû à des erreurs mineures et inévitables de précision lors de la conversion vers le mode caractère et à partir de ce mode.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter **sp_table_validation**, vous devez disposer des autorisations SELECT sur la table en cours de validation.  
  
## <a name="see-also"></a>Voir aussi  
 [SOMME de contrôle &#40;&#41;Transact-SQL ](../../t-sql/functions/checksum-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)   
 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
