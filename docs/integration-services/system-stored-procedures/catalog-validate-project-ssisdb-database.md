---
description: catalog.validate_project (base de données SSISDB)
title: catalog.validate_project (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5270689a-46d4-4847-b41f-3bed1899e955
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f57af2d5c29af5d3fd904c04637c0ba585b107a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355294"
---
# <a name="catalogvalidate_project-ssisdb-database"></a>catalog.validate_project (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Valide de façon asynchrone un projet dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql
catalog.validate_project [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @validate_type = ] validate_type  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @folder_name = ] *folder_name*  
 Nom d'un dossier qui contient le projet. *folder_name* est de type **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Nom du projet. *project_name* est de type **nvarchar(128)** .  
  
 [ @validate_type = ] *validate_type*  
 Indique le type de validation à réaliser. Utilisez le caractère `F` pour effectuer une validation complète. Ce paramètre est facultatif, le caractère `F` sera utilisé par défaut. *validate_type* est de type **char(1)**.  
  
 [ @validation_id = ] *validation_id*  
 Retourne l'identificateur unique (ID) de la validation. *validation_id* est de type **bigint**.  
  
 [ @use32bitruntime = ] *use32bitruntime*  
 Indique si l'exécution 32 bits doit être utilisée pour exécuter le package sur un système d'exploitation 64 bits. Utilisez la valeur `1` pour exécuter le package avec l’exécution 32 bits quand un système d’exploitation 64 bits est exécuté. Utilisez la valeur `0` pour exécuter le package avec l'exécution 64 bits lorsqu'un système d'exploitation 64 bits est exécuté. Ce paramètre est facultatif. *use32bitruntime* est de type **bit**.  
  
 [ @environment_scope = ] *environment_scope*  
 Indique les références environnementales considérées par la validation. Lorsque la valeur est `A`, toutes les références environnementales associées au projet sont incluses dans la validation. Lorsque la valeur est `S`, seule une référence environnementale unique est incluse. Lorsque la valeur est `D`, aucune référence environnementale n'est incluse et chaque paramètre doit avoir une valeur par défaut littérale pour passer la validation. Ce paramètre est facultatif, le caractère `D` sera utilisé par défaut. *environment_scope* est de type **char(1)**.  
  
 [ @reference_id = ] *reference_id*  
 ID unique de la référence environnementale. Ce paramètre est obligatoire uniquement quand une référence environnementale unique est incluse dans la validation, quand *environment_scope* est `S`. *reference_id* est de type **bigint**.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 La sortie des étapes de validation est retournée sous la forme de sections différentes du jeu de résultats.  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ sur le projet et, si applicable, autorisations READ sur les environnements référencés  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante fournit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   La validation échoue pour un ou plusieurs packages dans le projet  
  
-   La validation échoue si un ou plusieurs des environnements référencés inclus dans la validation ne contiennent pas de variables référencées  
  
-   Le type validé spécifié n'est pas valide.  
  
-   Le nom du projet ou l'ID de référence d'environnement n'est pas valide  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
## <a name="remarks"></a>Notes  
 La validation aide à identifier les problèmes qui empêchent les packages dans le projet de s'exécuter avec succès. Utilisez les vues [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md) ou [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) pour surveiller l’état de validation.  
  
 Seuls les environnements qui sont accessibles par l'utilisateur peuvent être utilisés dans la validation. La sortie de validation est envoyée au client sous forme d'un jeu de résultats.  
  
 Dans cette version, la validation de projet ne prend pas en charge la validation de dépendance.  
  
 La validation complète confirme que toutes les variables d'environnement référencées sont recherchées dans les environnements référencés inclus dans la validation. Les résultats de la validation complète répertorient des références environnementales qui ne sont pas des variables d'environnement valides et référencées et qui sont introuvables dans les environnements référencés inclus dans la validation.  
  
  
