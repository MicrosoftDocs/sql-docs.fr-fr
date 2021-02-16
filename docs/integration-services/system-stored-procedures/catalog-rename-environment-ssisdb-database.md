---
description: catalog.rename_environment (base de données SSISDB)
title: catalog.rename_environment (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: c73d7452-31c5-4f4e-afcc-e9eca760c826
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5cff0cdf01fbfbb0af768a18ab54614c2a46f5bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346514"
---
# <a name="catalogrename_environment-ssisdb-database"></a>catalog.rename_environment (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renomme un environnement dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.rename_environment [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @new_environment_name= ] new_environment_name  
```  
  
## <a name="arguments"></a>Arguments  
 [ @folder_name = ] *folder_name*  
 Nom du dossier qui contient l'environnement. *folder_name* est de type **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Nom d'origine de l'environnement. *environment_name* est de type **nvarchar(128)** .  
  
 [ @new_environment_name = ] *new_environment_name*  
 Nouveau nom de l'environnement. *new_environment_name* est de type **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations MODIFY sur l'environnement  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   Le nom d'environnement d'origine n'est pas valide  
  
-   Le nouveau nom a déjà été utilisé dans un environnement existant  
  
## <a name="remarks"></a>Notes  
 Les références environnementales des projets ne sont pas mises à jour automatiquement lorsque vous renommez l'environnement. Les références environnementales doivent être mises à jour en conséquence. Cette procédure stockée réussira même si les références environnementales sont arrêtées en modifiant le nom d'un environnement. Les références environnementales doivent être mises à jour après que cette procédure stockée s'est achevée.  
  
> [!NOTE]  
>  Lorsqu'une référence environnementale n'est pas valide, la validation et l'exécution des packages correspondants qui utilisent ces références échoueront.  
  
  
