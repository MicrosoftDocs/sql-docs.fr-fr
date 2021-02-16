---
description: catalog.set_environment_reference_type (base de données SSISDB)
title: catalog.set_environment_reference_type (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: b79e3a06-22c0-40e5-8933-1b3414db3329
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9948b4cdc9c1c9fc6467ba718f3a1a7c59b78e54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273210"
---
# <a name="catalogset_environment_reference_type-ssisdb-database"></a>catalog.set_environment_reference_type (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit le type de référence et le nom de l'environnement associés à une référence environnementale existante pour un projet dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.set_environment_reference_location [ @reference_id = reference_id  
    , [ @reference_type = ] reference_type  
 [  , [ @environment_folder_name = ] environment_folder_name ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @reference_id = ] *reference_id*  
 Identificateur unique de la référence environnementale qui sera mise à jour. *reference_id* est de type **bigint**.  
  
 [ @reference_type = ] *reference_type*  
 Indique si l'environnement peut se trouver dans le même dossier que le projet (référence relative) ou dans un dossier différent (référence absolue). Utilisez la valeur `R` pour indiquer une référence relative. Utilisez la valeur `A` pour indiquer une référence absolue. *reference_type* est de type **char(1)** .  
  
 [ @environment_folder_name = ] *environment_folder_name*  
 Dossier dans lequel l'environnement se trouve. Cette valeur est obligatoire pour les références absolues. *environment_folder_name* est de type **nvarchar(128)** .  
  
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
  
-   Le nom du dossier, le nom de l’environnement ou l’ID de référence n’est pas valide  
  
-   L'utilisateur n'a pas les autorisations appropriées  
  
-   Une référence absolue est spécifiée en utilisant le caractère `A` dans le paramètre *reference_location*, mais le nom du dossier n’a pas été spécifié avec le paramètre *environment_folder_name*.  
  
## <a name="remarks"></a>Notes   
 Un projet peut avoir des références environnementales relatives ou absolues. Les références relatives font référence à l'environnement par nom et requièrent qu'il réside dans le même dossier que le projet. Les références absolues font référence à l'environnement par nom et par dossier et peuvent faire référence aux environnements qui résident dans un dossier différent que le projet. Un projet peut référencer plusieurs environnements.  
  
> [!IMPORTANT]  
>  Si une référence relative est spécifiée, la valeur de paramètre *environment_folder_name* n’est pas utilisée, et le nom du dossier d’environnement est défini automatiquement sur **NULL**. Si une référence absolue est spécifiée, le nom du dossier d’environnement doit être fourni dans le paramètre *environment_folder_name*.  
  
  
