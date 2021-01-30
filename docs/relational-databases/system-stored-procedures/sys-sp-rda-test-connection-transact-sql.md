---
title: sys.sp_rda_test_connection (Transact-SQL) | Microsoft Docs
description: Apprenez à utiliser sys.sp_rda_test_connection pour tester la connexion de SQL Server au serveur Azure distant et signaler des problèmes susceptibles d’empêcher la migration de données.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_test_connection
- sys.sp_rda_test_connection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_test_connection stored procedure
ms.assetid: e2ba050c-d7e3-4f33-8281-c9b525b4edb4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e584604a0229e3e2e0c213b70d8fd4bca0321c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211715"
---
# <a name="syssp_rda_test_connection-transact-sql"></a>sys.sp_rda_test_connection (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Teste la connexion de SQL Server au serveur Azure distant et signale les problèmes susceptibles d’empêcher la migration des données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
EXECUTE sys.sp_rda_test_connection  
   @database_name = N'db_name',   
   @server_address = N'azure_server_fully_qualified_address',  
   @azure_username = N'azure_username',   
   @azure_password = N'azure_password',  
   @credential_name = N'credential_name'  
  
```  
  
## <a name="arguments"></a>Arguments  
 @database_name = N'*db_name*'  
 Nom de la base de données SQL Server avec Stretch. Ce paramètre est facultatif.  
  
 @server_address = N'*azure_server_fully_qualified_address*'  
 Adresse complète du serveur Azure.  
  
-   Si vous fournissez une valeur pour **\@ database_name**, mais que la base de données spécifiée n’est pas compatible avec Stretch, vous devez fournir une valeur pour **\@ server_address**.  
  
-   Si vous fournissez une valeur pour **\@ database_name** et que la base de données spécifiée est compatible avec Stretch, vous n’avez pas besoin de fournir une valeur pour **\@ server_address**. Si vous fournissez une valeur pour **\@ server_address**, la procédure stockée l’ignore et utilise le serveur Azure existant déjà associé à la base de données Stretch.  
  
 @azure_username = N'*azure_username*  
 Nom d’utilisateur du serveur Azure distant.  
  
 @azure_password = N'*azure_password*'  
 Mot de passe du serveur Azure distant.  
  
 @credential_name = N'*credential_name*'  
 Au lieu de fournir un nom d’utilisateur et un mot de passe, vous pouvez fournir le nom d’une information d’identification stockée dans la base de données Stretch.  
  
## <a name="return-code-values"></a>Codet de retour  
 En cas de **réussite**, sp_rda_test_connection retourne l’erreur 14855 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_SUCCEEDED) avec la gravité EX_INFO et un code de retour de réussite.  
  
 En cas de **défaillance**, sp_rda_test_connection renvoie l’erreur 14856 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_FAILED) avec une gravité EX_USER et un code de retour d’erreur.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|link_state|int|L’une des valeurs suivantes, qui correspondent aux valeurs de **link_state_desc**.<br /><br /> -0<br />-1<br />-2<br />-3<br />-4|  
|link_state_desc|varchar(32)|L’une des valeurs suivantes, qui correspondent aux valeurs précédentes pour **LINK_STATE**.<br /><br /> -SAIN<br />     L’état entre SQL Server et le serveur Azure distant est sain.<br />-ERROR_AZURE_FIREWALL<br />     Le pare-feu Azure empêche le lien entre SQL Server et le serveur Azure distant.<br />-ERROR_NO_CONNECTION<br />     SQL Server ne peut pas établir de connexion au serveur Azure distant.<br />-ERROR_AUTH_FAILURE<br />     Un échec d’authentification empêche le lien entre SQL Server et le serveur Azure distant.<br />-ERREUR<br />     Une erreur qui n’est pas un problème d’authentification, un problème de connectivité ou un problème de pare-feu empêche le lien entre SQL Server et le serveur Azure distant.|  
|error_number|int|Numéro de l'erreur. S’il n’y a pas d’erreur, ce champ a la valeur NULL.|  
|error_message|nvarchar(1024)|Message d’erreur. S’il n’y a pas d’erreur, ce champ a la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert les autorisations db_owner.  
  
## <a name="examples"></a>Exemples  
  
### <a name="check-the-connection-from-sql-server-to-the-remote-azure-server"></a>Vérifier la connexion de SQL Server au serveur Azure distant  
  
```sql  
EXECUTE sys.sp_rda_test_connection @database_name = N'<Stretch-enabled database>'  
GO  
  
```  
  
 Les résultats indiquent que SQL Server ne peut pas se connecter au serveur Azure distant.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|2|ERROR_NO_CONNECTION|*\<connection-related error number>*|*\<connection-related error message>*|  
  
### <a name="check-the-azure-firewall"></a>Vérifier le pare-feu Azure  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 Les résultats indiquent que le pare-feu Azure empêche le lien entre SQL Server et le serveur Azure distant.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|1|ERROR_AZURE_FIREWALL|*\<firewall-related error number>*|*\<firewall-related error message>*|  
  
### <a name="check-authentication-credentials"></a>Vérifier les informations d’identification d’authentification  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 Les résultats montrent qu’un échec d’authentification empêche la liaison entre SQL Server et le serveur Azure distant.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|3|ERROR_AUTH_FAILURE|*\<authentication-related error number>*|*\<authentication-related error message>*|  
  
### <a name="check-the-status-of-the-remote-azure-server"></a>Vérifier l’état du serveur Azure distant  
  
```sql  
USE <SQL Server database>  
GO  
EXECUTE sys.sp_rda_test_connection   
    @server_address = N'<server name>.database.windows.net',   
    @azure_username = N'<user name>',   
    @azure_password = N'<password>'  
GO  
  
```  
  
 Les résultats indiquent que la connexion est intègre et que vous pouvez activer Stretch Database pour la base de données spécifiée.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|0|HEALTHY|NULL|NULL|  
  
  
