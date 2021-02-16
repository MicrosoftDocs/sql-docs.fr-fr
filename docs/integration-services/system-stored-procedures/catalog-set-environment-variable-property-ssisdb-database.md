---
description: catalog.set_environment_variable_property (base de données SSISDB)
title: catalog.set_environment_variable_property (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: c1deb31e-b8d1-44ca-b355-570959bc6478
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ac4302148038183ab5415bb8bc37050e1464a887
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273200"
---
# <a name="catalogset_environment_variable_property-ssisdb-database"></a>catalog.set_environment_variable_property (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit la propriété d'une variable d'environnement dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.set_environment_variable_property [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @property_name = ] property_name  
    , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Arguments  
 [ @folder_name = ] *folder_name*  
 Nom du dossier qui contient l'environnement. *folder_name* est de type **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Nom de l’environnement. *environment_name* est de type **nvarchar(128)** .  
  
 [ @variable_name = ] *variable_name*  
 Nom de la variable d’environnement. *variable_name* est de type **nvarchar(128)**.  
  
 [ @property_name = ] *property_name*  
 Nom de la propriété de variable d'environnement. *property_name* est de type **nvarchar(128)**.  
  
 [ @property_value = ] *property_value*  
 Valeur de la propriété de variable d'environnement. *property_value* est de type **nvarchar(4000)**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et MODIFY sur l'environnement  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   Le nom du dossier n'est pas valide.  
  
-   Le nom de l'environnement n'est pas valide.  
  
-   Le nom de la variable d'environnement n'est pas valide  
  
-   Le nom de la propriété de variable d'environnement n'est pas valide  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
## <a name="remarks"></a>Notes  
 Dans cette version finale, seule la propriété `Description` peut être définie. La valeur de la propriété `Description` ne peut pas dépasser 4000 caractères.  
  
  
