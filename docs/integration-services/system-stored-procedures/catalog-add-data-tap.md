---
description: catalog.add_data_tap
title: catalog.add_data_tap | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a25ebcc7-535e-4619-adf6-4e2b5a62ba37
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8d188b361b32627ffac604a9b69e14f54fa26355
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339198"
---
# <a name="catalogadd_data_tap"></a>catalog.add_data_tap 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un drainage de données sur la sortie d'un composant dans un flux de données de package, pour une instance d'exécution.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.add_data_tap [ @execution_id = ] execution_id  
, [ @task_package_path = ] task_package_path  
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
 [ @execution_id = ] *execution_id*  
 ID de l'exécution contenant le package. *execution_id* est de type **bigint**.  
  
 [ @task_package_path = ] *task_package_path*  
 Chemin d'accès de package de la tâche de flux de données. La propriété **PackagePath** de la tâche de flux de données spécifie le chemin d’accès. Le chemin d'accès respecte la casse. Pour rechercher le chemin d’accès au package, dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] cliquez avec le bouton droit sur la tâche de flux de données, puis cliquez sur **Propriétés**. La propriété **PackagePath** s’affiche dans la fenêtre **Propriétés**.  
  
 *task_package_path* est de type **nvarchar(max)** .  
  
 [ @dataflow_path_id_string = ] *dataflow_path_id_string*  
 Chaîne d’identification du chemin d’accès de flux de données. Un chemin d'accès connectent deux composants de flux de données. La propriété **IdentificationString** du chemin d’accès spécifie la chaîne.  
  
 Pour rechercher la chaîne d’identification, dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] cliquez avec le bouton droit sur le chemin d’accès entre deux composants de flux de données, puis cliquez sur **Propriétés**. La propriété **IdentificationString** s’affiche dans la fenêtre **Propriétés**.  
  
 *dataflow_path_id_string* est de type **nvarchar(4000)** .  
  
 [ @data_filename = ] *data_filename*  
 Nom du fichier qui stocke les données drainées. Si la tâche de flux de données s'exécute à l'intérieur d'un conteneur de boucles Foreach ou For, des fichiers distincts stockent les données drainées pour chaque itération de la boucle. Chaque fichier a pour préfixe un nombre qui correspond à une itération.  
  
 Par défaut, le fichier est stocké dans le dossier \<*drive*>:\Program Files\Microsoft SQL Server\130\DTS\DataDumps.  
  
 *data_filename* est de type **nvarchar(4000)** .  
  
 [ @max_rows = ] *max_rows*  
 Nombre de lignes capturées pendant le drainage de données. Si cette valeur n'est pas spécifiée, toutes les lignes sont capturées. *max_rows* est de type **int**.  
  
 [ @data_tap_id = ] *data_tap_id*  
 Retourne l'ID de la collecte de données. *data_tap_id* est de type **bigint**.  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, un drainage de données est créé sur le chemin d’accès de flux de données, `'Paths[OLE DB Source.OLE DB Source Output]`, dans la tâche de flux de données, `\Package\Data Flow Task`. Les données drainées sont stockées dans le fichier `output0.txt` dans le dossier DataDumps (\<*drive*>:\Program Files\Microsoft SQL Server\130\DTS\DataDumps).  
  
```sql
Declare @execution_id bigint  
Exec SSISDB.Catalog.create_execution @folder_name='Packages',@project_name='SSISPackages', @package_name='Package.dtsx',@reference_id=Null, @use32bitruntime=False, @execution_id=@execution_id OUTPUT  
  
Exec SSISDB.Catalog.set_execution_parameter_value @execution_id,50, 'LOGGING_LEVEL', 0  
  
Exec SSISDB.Catalog.add_data_tap @execution_id, @task_package_path='\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[OLE DB Source.OLE DB Source Output]', @data_filename = 'output0.txt'  
  
Exec SSISDB.Catalog.start_execution @execution_id  
```  
  
## <a name="remarks"></a>Notes  
 Pour ajouter des drainages de données, l’instance d’exécution doit avoir l’état Created (valeur 1 dans la colonne **status** de la vue [catalog.operations &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)). La valeur d'état change lorsque vous exécutez l'exécution. Vous pouvez créer une exécution en appelant [catalog.create_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
 Les considérations suivantes sont à prendre en compte pour la procédure stockée add_data_tap.  
  
-   Si une exécution contient un package parent et un ou plusieurs packages enfants, vous devez ajouter un drainage de données pour chaque package pour lequel vous souhaitez effectuer un drainage de données.  
  
-   Si un package contient plusieurs tâches de flux de données portant le même nom, task_package_path identifie de manière unique la tâche de flux de données qui contient la sortie du composant drainé.  
  
-   Lorsque vous ajoutez une collecte de données, celle-ci n'est pas validée tant que le package n'est pas exécuté.  
  
-   Il est recommandé de limiter le nombre de lignes capturées pendant le drainage des données, pour éviter de générer des fichiers de données de grande taille. Si l'ordinateur sur lequel la procédure stockée est exécutée, manque d'espace de stockage pour les fichiers de données, le package cesse de s'exécuter et un message d'erreur est consigné dans un journal.  
  
-   L'exécution de la procédure stockée add_data_tap affecte les performances du package. Il est recommandé d'exécuter la procédure stockée seulement pour résoudre des problèmes de données.  
  
-   Pour accéder au fichier qui stocke les données drainées, vous devez être administrateur sur l'ordinateur sur lequel la procédure stockée est exécutée. Vous devez également être l'utilisateur qui a démarré l'exécution contenant le package avec le drainage de données.  
  
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
  
## <a name="external-resources"></a>Ressources externes  
 Entrée de blog, [SSIS 2012 : aperçu de Data Taps](https://go.microsoft.com/fwlink/?LinkId=239983) sur le site rafael-salas.com.  
  
## <a name="see-also"></a>Voir aussi  
 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)  
  
  
