---
description: catalog.add_data_tap_by_guid
title: catalog.add_data_tap_by_guid | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ed9d7fa3-61a1-4e21-ba43-1ead7dfc74eb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 969f6975d2f73cfea80582694a3b0370a00f7264
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354914"
---
# <a name="catalogadd_data_tap_by_guid"></a>catalog.add_data_tap_by_guid 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un drainage de données à un chemin de flux de données spécifique dans un flux de données de package, pour une instance d'exécution.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.add_data_tap_by_guid [ @execution_id = ] execution_id  
, [ @dataflow_task_guid = ] dataflow_task_guid   
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id  
```  
  
## <a name="arguments"></a>Arguments  
 [ @execution_id = ] *execution_id*  
 ID de l'exécution contenant le package. *execution_id* est de type **bigint**.  
  
 [ @dataflow_task_guid = ] *dataflow_task_guid*  
 ID du flux de tâches de données dans le package qui contient le chemin d'accès au flux de données à drainer. *dataflow_task_guid* est de type **uniqueidentifier**.  
  
 [ @dataflow_path_id_string = ] *dataflow_path_id_string*  
 Chaîne d’identification du chemin d’accès de flux de données. Un chemin d'accès connectent deux composants de flux de données. La propriété **IdentificationString** du chemin d’accès spécifie la chaîne.  
  
 Pour rechercher la chaîne d’identification, dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] cliquez avec le bouton droit sur le chemin d’accès entre deux composants de flux de données, puis cliquez sur **Propriétés**. La propriété **IdentificationString** s’affiche dans la fenêtre **Propriétés**.  
  
 *dataflow_path_id_string* est de type **nvarchar(4000)** .  
  
 [ @data_filename = ] *data_filename*  
 Nom du fichier qui stocke les données drainées. Si la tâche de flux de données s'exécute à l'intérieur d'un conteneur de boucles Foreach ou For, des fichiers distincts stockent les données drainées pour chaque itération de la boucle. Chaque fichier a pour préfixe un nombre qui correspond à une itération. Les fichiers de drainage de données sont écrits dans le dossier « *\<SQL Server installation folder>* \130\DTS\\ ». *data_filename* est de type **nvarchar(4000)** .  
  
 [ @max_rows = ] max_rows  
 Nombre de lignes capturées pendant le drainage de données. Si cette valeur n'est pas spécifiée, toutes les lignes sont capturées. max_rows est de type **int**.  
  
 [ @data_tap_id = ] *data_tap_id*  
 ID de la collecte de données. *data_tap_id* est de type **bigint**.  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, un drainage de données est créé sur le chemin d’accès de flux de données, `Paths[SRC DimDCVentor.OLE DB Source Output]`, dans la tâche de flux de données `{D978A2E4-E05D-4374-9B05-50178A8817E8}`. Les données drainées sont stockées dans le fichier DCVendorOutput.csv.  
  
```sql
exec catalog.add_data_tap_by_guid   @execution_id,   
'{D978A2E4-E05D-4374-9B05-50178A8817E8}',   
'Paths[SRC DimDCVentor.OLE DB Source Output]',   
'D:\demos\datafiles\DCVendorOutput.csv'  
```  
  
## <a name="remarks"></a>Notes  
 Pour ajouter des drainages de données, l’instance d’exécution doit avoir l’état Created (valeur 1 dans la colonne **status** de la vue [catalog.operations &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)). La valeur d'état change lorsque vous exécutez l'exécution. Vous pouvez créer une exécution en appelant [catalog.create_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
 Les considérations suivantes sont à prendre en compte pour la procédure stockée add_data_tap_by_guid.  
  
-   Lorsque vous ajoutez un drainage de données, celui-ci n'est pas validé tant que le package n'est pas exécuté.  
  
-   Il est recommandé de limiter le nombre de lignes capturées pendant le drainage des données, pour éviter de générer des fichiers de données de grande taille. Si l'ordinateur sur lequel la procédure stockée est exécutée ne dispose pas d'assez d'espace de stockage pour les fichiers de données, la procédure arrête l'exécution.  
  
-   L'exécution de la procédure stockée add_data_tap_by_guid affecte les performances du package. Il est recommandé d'exécuter la procédure stockée seulement pour résoudre des problèmes de données.  
  
-   Pour accéder au fichier qui stocke les données drainées, vous devez disposer d'autorisations d'administrateur sur l'ordinateur où la procédure stockée est exécutée, ou vous devez être l'utilisateur qui a lancé l'exécution qui contient le package avec le drainage de données.  
  
## <a name="return-codes"></a>Codes de retour  
 0 (succès)  
  
 Lorsque la procédure stockée échoue, elle génère une erreur.  
  
## <a name="result-set"></a>Jeu de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations MODIFY sur l'instance d'exécution  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit les conditions provoquant l'échec de la procédure stockée.  
  
-   L'utilisateur ne dispose pas des autorisations MODIFY.  
  
-   Le drainage de données du composant spécifié, dans le package spécifié, a déjà été ajouté.  
  
-   La valeur spécifiée pour le nombre de lignes à capturer n'est pas valide.  
  
## <a name="requirements"></a>Spécifications  
  
## <a name="see-also"></a>Voir aussi  
 [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md)  
  
  
