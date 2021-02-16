---
description: catalog.configure_catalog (base de données SSISDB)
title: catalog.configure_catalog (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 72690c61-f462-4c25-9fce-08a687b0bd41
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5d52f2ea964433863f735fff505b1d80bb2e3fc8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346857"
---
# <a name="catalogconfigure_catalog-ssisdb-database"></a>catalog.configure_catalog (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configure le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en définissant une propriété de catalogue sur une valeur spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql
catalog.configure_catalog [ @property_name = ] property_name , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Arguments  
 [ @property_name = ] *property_name*  
 Nom de la propriété de catalogue. *property_name* est de type **nvarchar(255)**. Pour plus d’informations sur les propriétés disponibles, consultez [catalog.catalog_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
 [ @property_value = ] *property_value*  
 Valeur de la propriété de catalogue. *property_value* est de type **nvarchar(255)**. Pour plus d’informations sur les valeurs de propriété, consultez [catalog.catalog_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Cette procédure stockée détermine si le *property_value* est valide pour chaque *property_name*.  
  
 Cette procédure stockée peut être effectuée uniquement lorsqu'il n'y a pas d'exécution active, c'est-à-dire pas d'exécutions en attente, en file d'attente, en cours d'exécution ou suspendues.  
  
 Pendant que le catalogue est configuré, toutes les autres procédures stockées du catalogue échouent avec le message d’erreur « La configuration du serveur est en cours ».
  
 Lorsque le catalogue est configuré, une entrée est écrite dans le journal des opérations.  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   La propriété n'existe pas  
  
-   La valeur de la propriété n'est pas valide  
  
  
