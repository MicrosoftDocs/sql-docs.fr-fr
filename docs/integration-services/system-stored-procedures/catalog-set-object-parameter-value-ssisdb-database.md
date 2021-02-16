---
description: catalog.set_object_parameter_value (base de données SSISDB)
title: catalog.set_object_parameter_value (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: fb887543-f92f-404d-9495-a1dd23a6716e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bdc1a5ff8944a29d7e2a2fcdeec297973c9849f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355306"
---
# <a name="catalogset_object_parameter_value-ssisdb-database"></a>catalog.set_object_parameter_value (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit la valeur d'un paramètre dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Associe la valeur à une variable d’environnement ou affecte une valeur littérale qui est utilisée par défaut quand aucune autre valeur n’est affectée.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.set_object_parameter_value [ @object_type = ] object_type   
    , [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @parameter_name = ] parameter_name   
    , [ @parameter_value = ] parameter_value   
 [  , [ @object_name = ] object_name ]  
 [  , [ @value_type = ] value_type ]  
```  
  
## <a name="arguments"></a>Arguments  
 [@object_type =] *object_type*  
 Type de paramètre. Utilisez la valeur `20` pour indiquer un paramètre du projet ou la valeur `30` pour indiquer un paramètre du package. *object_type* est de type **smallInt**.  
  
 [@folder_name =] *folder_name*  
 Nom du dossier qui contient le paramètre. *folder_name* est de type **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Nom du projet qui contient le paramètre. *project_name* est de type **nvarchar(128)** .  
  
 [@parameter_name =] *parameter_name*  
 Le nom du paramètre. *parameter_name* est de type **nvarchar(128)**.  
  
 [@parameter_value =] *parameter_value*  
 Valeur du paramètre. *parameter_value* est de type **sql_variant**.  
  
 [@object_name =] *object_name*  
 Nom du package. Cet argument est obligatoire lorsque le paramètre est un paramètre du package. *object_name* est de type **nvarchar(260)**.  
  
 [@value_type =] *value_type*  
 Type de valeur du paramètre. Utilisez le caractère `V` pour indiquer que *parameter_value* est une valeur littérale qui est utilisée par défaut quand aucune autre valeur n’est affectée avant l’exécution. Utilisez le caractère `R` pour indiquer que *parameter_value* est une valeur référencée et qu’elle a été définie sur le nom d’une variable d’environnement. Cet argument est facultatif, le caractère `V` est utilisé par défaut. *value_type* est de type **char(1)**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et MODIFY sur le projet  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur de procédure stockée :  
  
-   Le type de paramètre n'est pas valide.  
  
-   Le nom du projet n'est pas valide.  
  
-   Pour les paramètres du package, le nom du package n'est pas valide  
  
-   Le type de valeur n'est pas valide.  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
## <a name="remarks"></a>Notes  
  
-   Si aucun *value_type* n’est spécifié, une valeur littérale pour *parameter_value* est utilisée par défaut. Quand une valeur littérale est utilisée, le *value_set* dans la vue [object_parameters](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md) est défini sur `1`. Une valeur de paramètre NULL n'est pas autorisée.  
  
-   Si *value_type* contient le caractère `R`, qui dénote une valeur référencée, *parameter_value* fait référence au nom d’une variable d’environnement.  
  
-   La valeur `20` peut être utilisée pour *object_type* afin de dénoter un paramètre du projet. Dans ce cas, une valeur pour *object_name* n’est pas nécessaire, et toute valeur spécifiée pour *object_name* est ignorée. Cette valeur est utilisée lorsque l'utilisateur souhaite définir un paramètre du projet.  
  
-   La valeur `30` peut être utilisée pour *object_type* afin de dénoter un paramètre du package. Dans ce cas, une valeur pour *object_name* est utilisée pour dénoter le package correspondant. Si *object_name* n’est pas spécifié, la procédure stockée retourne une erreur et termine.  
  
  
