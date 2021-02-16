---
description: catalog.start_execution (base de données SSISDB)
title: catalog.start_execution (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: f8663ff3-aa98-4dd8-b850-b21efada0b87
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e98650614583eab2b40e6433721a7cf2d73b1a6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342168"
---
# <a name="catalogstart_execution-ssisdb-database"></a>catalog.start_execution (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Démarre une instance d'exécution dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.start_execution [ @execution_id = ] execution_id [, [ @retry_count = ] retry_count]  
```  
  
## <a name="arguments"></a>Arguments  
 [@execution_id =] *execution_id*  
 Identificateur unique de l'instance d'exécution. *execution_id* est de type **bigint**.
 
 [@retry_count =] *retry_count*  
 Nombre de nouvelles tentatives en cas d’échec de l’exécution. Il prend effet uniquement si l’exécution a lieu dans Scale Out. Ce paramètre est facultatif. S’il n’est pas spécifié, sa valeur est définie sur 0. *retry_count* est de type **int**.
  
## <a name="remarks"></a>Remarques  
 Une exécution est utilisée pour spécifier les valeurs de paramètre qui sont utilisées par un package pendant une instance d’exécution unique du package. Le projet correspondant peut être redéployé une fois une instance d'exécution créée et avant son démarrage. Dans ce cas, l’instance d’exécution référence un projet obsolète. Cette référence non valide provoque l’échec de la procédure stockée.  
  
> [!NOTE]  
>  Les exécutions peuvent être démarrées uniquement une fois. Pour démarrer une instance d’exécution, elle doit avoir l’état Created (valeur `1` dans la colonne **status** de la vue [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md)).  
  
## <a name="example"></a> Exemple  
 L'exemple suivant appelle catalog.create_execution pour créer une instance d'exécution pour le package Child1.dtsx. Project1 Integration Services contient le package. L'exemple appelle catalog.set_execution_parameter_value afin de définir des valeurs pour les paramètres Parameter1, Parameter2 et LOGGING_LEVEL. L'exemple appelle catalog.start_execution pour démarrer une instance d'exécution.  
  
```sql
Declare @execution_id bigint  
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Child1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'TestDeply4', @project_name=N'Integration Services Project1', @use32bitruntime=False, @reference_id=Null  
Select @execution_id  
DECLARE @var0 sql_variant = N'Child1.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter1', @parameter_value=@var0  
DECLARE @var1 sql_variant = N'Child2.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter2', @parameter_value=@var1  
DECLARE @var2 smallint = 1  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var2  
EXEC [SSISDB].[catalog].[start_execution] @execution_id  
GO  
```  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et MODIFY sur l'instance d'exécution, autorisations READ et EXECUTE sur le projet, et si applicable, autorisations READ sur l'environnement référencé  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
-   L'identificateur d'exécution n'est pas valide.  
  
-   L'exécution a déjà démarré, ou a déjà été effectuée ; les exécutions peuvent être démarrées une seule fois  
  
-   La référence environnementale associée au projet n'est pas valide  
  
-   Les valeurs de paramètre obligatoires n'ont pas été définies  
  
-   La version du projet associée à l'instance d'exécution est obsolète ; seule la version la plus actuelle d'un projet peut être exécutée  
  
  
