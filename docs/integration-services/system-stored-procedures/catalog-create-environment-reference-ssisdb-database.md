---
description: catalog.create_environment_reference (base de données SSISDB)
title: catalog.create_environment_reference (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 48069bea-31cb-4a0e-9849-a07edc94088f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: dcca4fd25fd201c9b278107dce188b6cab12d866
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88477109"
---
# <a name="catalogcreate_environment_reference-ssisdb-database"></a>catalog.create_environment_reference (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée une référence environnementale pour un projet dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.create_environment_reference [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @environment_name = ] environment_name  
     , [ @reference_type = ] reference_type  
  [  , [ @environment_folder_name = ] environment_folder_name ]  
  [  , [ @reference_id = ] reference_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @folder_name = ] *folder_name*  
 Nom du dossier du projet qui référence l'environnement. *folder_name* est de type **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Nom du projet qui référence l'environnement. *project_name* est de type **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Nom de l'environnement référencé. *environment_name* est de type **nvarchar(128)** .  
  
 [ @reference_type = ] *reference_type*  
 Indique si l'environnement peut se trouver dans le même dossier que le projet (référence relative) ou dans un dossier différent (référence absolue). Utilisez la valeur `R` pour indiquer une référence relative. Utilisez la valeur `A` pour indiquer une référence absolue. *reference_type* est de type **char(1)** .  
  
 [ @environment_folder_name = ] *environment_folder_name*  
 Nom du dossier dans lequel l'environnement référencé se trouve. Cette valeur est obligatoire pour les références absolues. *environment_folder_name* est de type **nvarchar(128)** .  
  
 [ @reference_id = ] *reference_id*  
 Retourne l'identificateur unique de la nouvelle référence. Ce paramètre est facultatif. *reference_id* est de type **bigint**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et MODIFY sur le projet, et autorisation READ sur l'environnement  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   Le nom du dossier n'est pas valide.  
  
-   Le nom du projet n'est pas valide.  
  
-   L'utilisateur n'a pas les autorisations appropriées  
  
-   Une référence absolue est spécifiée en utilisant le caractère `A` dans le paramètre *reference_location*, mais le nom du dossier n’a pas été spécifié avec le paramètre *environment_folder_name*.  
  
## <a name="remarks"></a>Notes   
 Un projet peut avoir des références environnementales relatives ou absolues. Les références relatives font référence à l'environnement par nom et requièrent qu'il réside dans le même dossier que le projet. Les références absolues font référence à l'environnement par nom et par dossier et peuvent faire référence aux environnements qui résident dans un dossier différent que le projet. Un projet peut référencer plusieurs environnements.  
  
  
