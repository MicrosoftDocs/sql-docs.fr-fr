---
description: catalog.set_execution_parameter_value (base de données SSISDB)
title: catalog.set_execution_parameter_value (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 055d86c9-befd-4e63-acb1-6dfe833549d2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c8c294aa9aefa326ad9b8d84ddb599f99f181cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273170"
---
# <a name="catalogset_execution_parameter_value-ssisdb-database"></a>catalog.set_execution_parameter_value (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit la valeur d'un paramètre pour une instance d'exécution dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Une valeur de paramètre ne peut pas être modifiée après qu'une instance d'exécution a commencé.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.set_execution_parameter_value [ @execution_id = execution_id  
    , [ @object_type = ] object_type  
    , [ @parameter_name = ] parameter_name  
    , [ @parameter_value = ] parameter_value  
```  
  
## <a name="arguments"></a>Arguments  
 [ @execution_id = ] *execution_id*  
 Identificateur unique de l'instance d'exécution. *execution_id* est de type **bigint**.  
  
 [ @object_type = ] *object_type*  
 Type de paramètre.  
  
 Pour les paramètres suivants, affectez la valeur 50 à *object_type*  
  
-   LOGGING_LEVEL  
  
-   CUSTOMIZED_LOGGING_LEVEL  
  
-   DUMP_ON_ERROR  
  
-   DUMP_ON_EVENT  
  
-   DUMP_EVENT_CODE  
  
-   CALLER_INFO  
  
-   SYNCHRONIZED  
  
 Utilisez la valeur `20` pour indiquer un paramètre du projet ou la valeur `30` pour indiquer un paramètre du package.  
  
 *object_type* est de type **smallint**.  
  
 [ @parameter_name = ] *parameter_name*  
 Le nom du paramètre. *parameter_name* est de type **nvarchar(128)**.  
  
 [ @parameter_value = ] *parameter_value*  
 Valeur du paramètre. *parameter_value* est de type **sql_variant**.  
  
## <a name="remarks"></a>Notes  
 Pour découvrir les valeurs de paramètre utilisées pour une exécution donnée, interrogez la vue catalog.execution_parameter_values.  
  
 Pour spécifier l’étendue des informations enregistrées durant une exécution de package, attribuez au paramètre *parameter_name* la valeur LOGGING_LEVEL et au paramètre *parameter_value* l’une des valeurs suivantes.  
  
 Définissez le paramètre *object_type* sur 50.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|None<br /><br /> La journalisation est désactivée. Seul l'état d'exécution du package est enregistré.|  
|1|De base<br /><br /> Tous les événements sont enregistrés, sauf les événements personnalisés et de diagnostic. Il s’agit de la valeur par défaut.|  
|2|Performances<br /><br /> Seules les statistiques de performances, et les événements OnError et OnWarning, sont enregistrés.|  
|3|Commentaires<br /><br /> Tous les événements sont enregistrés, y compris les événements personnalisés et de diagnostic. <br />Les événements personnalisés sont notamment ces événements consignés par les tâches Integration Services. Pour plus d’informations, consultez [Messages personnalisés pour la journalisation](../../integration-services/performance/integration-services-ssis-logging.md#custom_messages).|  
|4|Lignage de l’exécution<br /><br /> Collecte les données nécessaires au suivi du lignage dans le flux de données.|  
|100|Niveau de journalisation personnalisée<br /><br /> Spécifiez les valeurs du paramètre CUSTOMIZED_LOGGING_LEVEL. Pour plus d’informations sur les valeurs que vous pouvez spécifier, consultez [catalog.create_customized_logging_level](../../integration-services/system-stored-procedures/catalog-create-customized-logging-level.md).<br /><br /> Pour plus d’informations sur les niveaux de journalisation personnalisée, consultez [Activer la journalisation des exécutions de package sur le serveur SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).|  
  
 Pour spécifier que le serveur Integration Services doit générer des fichiers de vidage quand une erreur se produit pendant une exécution de package, définissez les valeurs de paramètre suivantes pour une instance d’exécution qui n’a pas été exécutée.  
  
|Paramètre|Valeur|  
|---------------|-----------|  
|*execution_id*|Identificateur unique de l'instance d'exécution|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_ERROR|  
|*parameter_value*|1|  
  
 Pour spécifier que le serveur Integration Services doit générer des fichiers de vidage quand des événements se produisent pendant une exécution de package, définissez les valeurs de paramètre suivantes pour une instance d’exécution qui n’a pas été exécutée.  
  
|Paramètre|Valeur|  
|---------------|-----------|  
|*execution_id*|Identificateur unique de l'instance d'exécution|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_EVENT|  
|*parameter_value*|1|  
  
 Pour spécifier les événements pendant une exécution de package qui provoquent la génération de fichiers de vidage par le serveur Integration Services, définissez les valeurs de paramètre suivantes pour une instance d’exécution qui n’a pas été exécutée. Séparez plusieurs codes d'événement à l'aide d'un point-virgule.  
  
|Paramètre|Valeur|  
|---------------|-----------|  
|*execution_id*|Identificateur unique de l'instance d'exécution|  
|*object_type*|50|  
|*parameter_name*|DUMP_EVENT_CODE|  
|*parameter_value*|Un ou plusieurs codes d'événement|  
  
## <a name="examples"></a>Exemples  

### <a name="a-generate-dump-files-for-errors"></a>R. Générer des fichiers de vidage pour les erreurs

 L'exemple suivant spécifie que le serveur Integration Services doit générer des fichiers de vidage lorsqu'une erreur se produit pendant une exécution de package.  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_ERROR',1  
```  
  
### <a name="b-generate-dump-files-for-events"></a>B. Générer des fichiers de vidage pour les événements

 L'exemple suivant spécifie que le serveur Integration Services doit générer des fichiers de vidage lorsque des événements se produisent pendant une exécution de package, et identifie l'événement qui provoque la génération des fichiers par le serveur.  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_EVENT',1  
  
declare @event_code nvarchar(50)  
set @event_code = '0xC020801C'  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_EVENT_CODE', @event_code  
```  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et MODIFY sur l'instance d'exécution  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   L’utilisateur n’a pas les autorisations appropriées  
  
-   L'identificateur d'exécution n'est pas valide.  
  
-   Le nom du paramètre n'est pas valide.  
  
-   Le type de données de la valeur de paramètre ne correspond pas au type du paramètre.  
  
## <a name="see-also"></a>Voir aussi  
 [catalog.execution_parameter_values &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)   
 [Générer de fichiers de vidage pour l’exécution des packages](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
  
