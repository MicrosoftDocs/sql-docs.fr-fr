---
description: catalog.move_project (base de données SSISDB)
title: catalog.move_project (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ef3b0325-d8e9-472b-bf11-7d3efa6312ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4c7cf1e3c23eacbabfd59ba3b8948740dc1a0543
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342193"
---
# <a name="catalogmove_project---ssisdb-database"></a>catalog.move_project - base de données SSISDB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Déplace un projet d'un dossier vers un autre dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.move_project [ @source_folder = ] source_folder  
    , [ @project_name = ] project_name  
    , [ @destination_folder = ] destination_folder  
```  
  
## <a name="arguments"></a>Arguments  
 [ @source_folder = ] *source_folder*  
 Nom du dossier source, où le projet réside avant le déplacement. *source_folder* est de type **nvarchar(128)**.  
  
 [ @project_name = ] *project_name*  
 Nom du projet qui sera supprimé. *project_name* est de type **nvarchar(128)** .  
  
 [ @destination_folder = ] *destination_folder*  
 Nom du dossier de destination, où le projet réside après le déplacement. *destination_folder* est de type **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et MODIFY sur le projet que vous souhaitez déplacer et autorisation CREATE_OBJECTS sur le dossier de destination  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur de cette procédure stockée :  
  
-   Le projet n'existe pas  
  
-   Le dossier source n'existe pas  
  
-   Le dossier de destination n'existe pas ou le dossier de destination contient déjà un projet avec le même nom  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
## <a name="remarks"></a>Notes  
 Lorsqu'un projet est déplacé d'un dossier source vers un dossier de destination, le projet dans le dossier source et les références environnementales correspondantes sont supprimés. Dans le dossier de destination, un projet et des références environnementales identiques sont créés. Les références environnementales relatives seront résolues dans un dossier différent après le déplacement. Les références absolues seront résolues dans le même dossier après le déplacement.  
  
> [!NOTE]  
>  Un projet peut avoir des références environnementales relatives ou absolues. Les références relatives font référence à l'environnement par nom et requièrent que l'environnement réside dans le même dossier que le projet. Les références absolues font référence à l'environnement par nom et dossier, et aux environnements qui résident dans un dossier différent de celui du projet.  
  
  
