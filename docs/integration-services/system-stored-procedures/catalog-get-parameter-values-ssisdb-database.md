---
description: catalog.get_parameter_values (base de données SSISDB)
title: catalog.get_parameter_values (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5b1aeaf7-c938-4aef-bafc-e4d7a82eb578
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 176fde267c490d41a41003e58cf1e201862a110b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342243"
---
# <a name="catalogget_parameter_values-ssisdb-database"></a>catalog.get_parameter_values (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Résout et extrait les valeurs de paramètre par défaut d'un projet et les packages correspondants dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.get_parameter_values [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @package_name = ] package_name  
  [  , [ @reference_id = ] reference_id  ]  
  
```  
  
## <a name="arguments"></a>Arguments  
 [ @folder_name = ] *folder_name*  
 Nom du dossier qui contient le projet. *folder_name* est de type **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Nom du projet où les paramètres résident. *project_name* est de type **nvarchar(128)** .  
  
 [ @package_name = ] *package_name*  
 Nom du package. Spécifiez le nom du package pour extraire tous les paramètres du projet et les paramètres d'un package spécifique. *package_name* est de type **nvarchar(260)**.  
  
 [ @reference_id = ] *reference_id*  
 Identificateur unique d’une référence environnementale. Ce paramètre est facultatif. *reference_id* est de type **bigint**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne une table qui a le format suivant :  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_type|**smallint**|Type de paramètre. La valeur est `20` pour un paramètre du projet et `30` pour un paramètre du package.|  
|parameter_data_type|**nvarchar(128)**|Type de données du paramètre.|  
|parameter_name|**sysname**|Le nom du paramètre.|  
|parameter_value|**sql_variant**|Valeur du paramètre.|  
|sensible|**bit**|Lorsque la valeur est `1`, la valeur de paramètre est sensible. Lorsque la valeur est `0`, la valeur de paramètre n'est pas sensible.|  
|obligatoire|**bit**|Lorsque la valeur est `1`, la valeur de paramètre est obligatoire pour démarrer l'exécution. Lorsque la valeur est `0`, la valeur de paramètre n'est pas obligatoire pour démarrer l'exécution.|  
|value_set|**bit**|Lorsque la valeur est `1`, la valeur de paramètre a été affectée. Lorsque la valeur est `0`, la valeur de paramètre n'a pas été affectée.|  
  
> [!NOTE]  
>  Les valeurs littérales sont affichées en texte en clair. **NULL** est affiché au lieu des valeurs sensibles.  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ sur le projet et, si applicable, autorisations READ sur les environnements référencés  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   Le package est introuvable dans le dossier ou le projet spécifié  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
-   La référence environnementale spécifiée n'existe pas  
  
  
