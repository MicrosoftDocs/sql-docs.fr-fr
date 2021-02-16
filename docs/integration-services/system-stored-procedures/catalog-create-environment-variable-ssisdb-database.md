---
description: catalog.create_environment_variable (base de données SSISDB)
title: catalog.create_environment_variable (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91ed017b-6567-4bf2-b9f1-e2b5c70a5343
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7a3b2b55c5a0efbc2acf8152b89a292adf4cc298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346815"
---
# <a name="catalogcreate_environment_variable-ssisdb-database"></a>catalog.create_environment_variable (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée une variable d'environnement dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.create_environment_variable [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @data_type = ] data_type  
    , [ @sensitive = ] sensitive  
    , [ @value = ] value  
    , [ @description = ] description  
```  
  
## <a name="arguments"></a>Arguments  
 [@folder_name =] *folder_name*  
 Nom du dossier qui contient l'environnement. *folder_name* est de type **nvarchar(128)** .  
  
 [@environment_name =] *environment_name*  
 Nom de l’environnement. *environment_name* est de type **nvarchar(128)** .  
  
 [@variable_name =] *variable_name*  
 Nom de la variable d’environnement. *variable_name* est de type **nvarchar(128)**.  
  
 [@data_type =] *data_type*  
 Type de données de la variable. Les types de données de variable d’environnement pris en charge incluent **Boolean**, **Byte**, **DateTime**, **Double**, **Int16**, **Int32**, **Int64**, **Single**, **String**, **UInt32** et **UInt64**. Les types de données de variable d’environnement non pris en charge incluent **Char**, **DBNull**, **Object** et **Sbyte**. Le type de données du paramètre *data_type* est **nvarchar (128)**.  
  
 [@sensitive =] *sensitive*  
 Indique si la variable contient une valeur sensible ou pas. Utilisez une valeur de `1` pour indiquer que la valeur de la variable d'environnement est sensible ou une valeur de `0` pour indiquer qu'elle n'est pas sensible. Une valeur sensible est chiffrée lorsqu'elle est stockée. Une valeur qui n’est pas sensible est stockée en texte en clair. *Sensitive* est de type **bit**.  
  
 [@value =] *value*  
 Valeur de la variable d’environnement. *value* est de type **sql_variant**.  
  
 [@description =] *description*  
 Description de la variable d'environnement. *value* est de type **nvarchar(1024)**.  
  
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
  
-   Le nom du dossier, le nom de l'environnement ou la variable d'environnement n'est pas valide  
  
-   Le nom de variable existe déjà dans l'environnement  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
## <a name="remarks"></a>Notes  
 Une variable d'environnement peut être utilisée pour affecter efficacement une valeur à un paramètre du projet ouà  un paramètre du package pour une utilisation dans l'exécution d'un package. Les variables d'environnement permettent d'organiser les valeurs de paramètre. Les noms de variable doivent être uniques dans un environnement.  
  
 La procédure stockée valide le type de données de la variable pour s'assurer qu'elle est prise en charge par le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
> [!TIP]  
>  Envisagez d’utiliser le type de données **Int16** dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] au lieu du type de données **Sbyte** non pris en charge.  
  
 La valeur passée à cette procédure stockée avec le paramètre *value* est convertie d’un type de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en un type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] selon le tableau suivant :  
  
|Type de données Integration Services|Type de données de SQL Server|  
|------------------------------------|--------------------------|  
|**Booléen**|**bit**|  
|**Byte**|**binary**, **varbinary**|  
|**DateTime**|**datetime**, **datetime2**, **datetimeoffset**, **smalldatetime**|  
|**Double**|Valeur numérique exacte : **decimal**, **numeric** ; Valeur numérique approchée : **float**, **real**|  
|**Int16**|**smallint**|  
|**Int32**|**int**|  
|**Int64**|**bigint**|  
|**Unique**|Valeur numérique exacte : **decimal**, **numeric** ; Valeur numérique approchée : **float**, **real**|  
|**Chaîne**|**varchar**, **nvarchar**, **char**|  
|**UInt32**|**int** (**int** est le mappage le plus proche de **Uint32**.)|  
|**UInt64**|**bigint** (**int** est le mappage le plus proche de **Uint64**.)|  
  
  
