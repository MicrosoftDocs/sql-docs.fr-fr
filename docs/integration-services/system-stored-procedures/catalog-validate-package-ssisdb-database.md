---
description: catalog.validate_package (base de données SSISDB)
title: catalog.validate_package (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- validate_package stored procedure [Integration Services]
- catalog.validate_package stored procedure [Integration Services]
ms.assetid: 0dc03df1-b793-408f-af4c-c11188729abf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2aca1e5c0b1ace9e1020b58464459415a8939ce1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350264"
---
# <a name="catalogvalidate_package-ssisdb-database"></a>catalog.validate_package (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Valide de façon asynchrone un package dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql
catalog.validate_package [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @package_name = ] package_name  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @folder_name = ] *folder_name*  
 Nom du dossier qui contient le package. *folder_name* est de type **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Nom du  projet qui contient le package. *project_name* est de type **nvarchar(128)** .  
  
 [ @package_name = ] *package_name*  
 Nom du package. *package_name* est de type **nvarchar(260)**.  
  
 [ @validation_id = ] *validation_id*  
 Retourne l'identificateur unique (ID) de la validation. *validation_id* est de type **bigint**.  
  
 [ @use32bitruntime = ] *use32bitruntime*  
 Indique si l'exécution 32 bits doit être utilisée pour exécuter le package sur un système d'exploitation 64 bits. Utilisez la valeur `1` pour exécuter le package avec l’exécution 32 bits quand un système d’exploitation 64 bits est exécuté. Utilisez la valeur `0` pour exécuter le package avec l'exécution 64 bits lorsqu'un système d'exploitation 64 bits est exécuté. Ce paramètre est facultatif. *use32bitruntime* est de type **bit**.  
  
 [ @environment_scope = ] *environment_scope*  
 Indique les références environnementales considérées par la validation. Lorsque la valeur est `A`, toutes les références environnementales associées au projet sont incluses dans la validation. Lorsque la valeur est `S`, seule une référence environnementale unique est incluse. Lorsque la valeur est `D`, aucune référence environnementale n'est incluse et chaque paramètre doit avoir une valeur par défaut littérale pour passer la validation. Ce paramètre est facultatif. Le caractère `D` est utilisé par défaut. *environment_scope* est de type **char(1)**.  
  
 [ @reference_id = ] *reference_id*  
 ID unique de la référence environnementale. Ce paramètre est obligatoire uniquement quand une référence environnementale unique est incluse dans la validation, quand *environment_scope* est `S`. *reference_id* est de type **bigint**.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ sur le projet et, si applicable, autorisations READ sur les environnements référencés  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   Le nom du projet ou le nom de package n'est pas valide  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
-   Un ou plusieurs environnements référencés inclus dans la validation ne contiennent pas de variables référencées  
  
-   La validation du package échoue  
  
-   L'environnement référencé n'existe pas  
  
-   Les variables référencées ne peuvent pas être trouvées dans les environnements référencés inclus dans la validation  
  
-   Les variables sont référencées dans les paramètres du package, mais aucun environnement référencé n'a été inclus dans la validation  
  
## <a name="remarks"></a>Notes  
 La validation aide à identifier les problèmes qui peuvent empêcher le package de s’exécuter avec succès. Utilisez les vues [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md) ou [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) pour surveiller l’état de validation.  
  
  
