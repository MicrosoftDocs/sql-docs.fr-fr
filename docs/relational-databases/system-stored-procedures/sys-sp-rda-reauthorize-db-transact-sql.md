---
title: sys.sp_rda_reauthorize_db (Transact-SQL) | Microsoft Docs
description: Découvrez comment utiliser sys.sp_rda_reauthorize_db pour restaurer la connexion authentifiée entre une base de données Stretch locale et une base de données distante.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sp_rda_reauthorize_db
- sp_rda_reauthorize_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reauthorize_db stored procedure
ms.assetid: f6f3e4b2-8c72-4d23-a5de-fe671ca5c5cd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f8585fb11e753f2ae1d2a28aed3567aa13d3456a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197589"
---
# <a name="syssp_rda_reauthorize_db-transact-sql"></a>sys.sp_rda_reauthorize_db (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Restaure la connexion authentifiée entre une base de données locale activée pour Stretch et la base de données distante.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_rda_reauthorize_db @credential = @credential, @with_copy = @with_copy [ , @azure_servername = @azure_servername, @azure_databasename = @azure_databasename ]  
```  
  
## <a name="arguments"></a>Arguments  
 @credential= *\@ informations d’identification*  
 Informations d’identification délimitées à la base de données associées à la base de données Stretch locale.  
  
 @with_copy= *\@ with_copy*  
 Spécifie s’il faut effectuer une copie des données distantes et se connecter à la copie (recommandé). *\@ with_copy* est de bits.  
  
 @azure_servername= *\@ azure_servername*  
 Spécifie le nom du serveur Azure qui contient les données distantes. *\@ azure_servername* est de type sysname.  
  
 @azure_databasename= *\@ azure_databasename*  
 Spécifie le nom de la base de données Azure qui contient les données distantes. *\@ azure_databasename* est de type sysname.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou >0 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Requiert les autorisations db_owner.  
  
## <a name="remarks"></a>Notes  
 Lorsque vous exécutez [sys.sp_rda_reauthorize_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) pour vous reconnecter à la base de données Azure distante, cette opération réinitialise automatiquement le mode de requête sur LOCAL_AND_REMOTE, qui est le comportement par défaut pour Stretch Database. Autrement dit, les requêtes retournent des résultats à partir de données locales et distantes.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant restaure la connexion authentifiée entre une base de données locale activée pour Stretch et la base de données distante. Il effectue une copie des données distantes (recommandé) et se connecte à la nouvelle copie.  
  
```sql  
DECLARE @credentialName nvarchar(128);   
SET @credentialName = N'<existing_database_scoped_credential_name>';   
EXEC sp_rda_reauthorize_db @credential = @credentialName, @with_copy = 1;  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_rda_deauthorize_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)   
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
