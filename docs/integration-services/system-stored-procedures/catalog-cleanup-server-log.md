---
description: catalog.cleanup_server_log
title: catalog.cleanup_server_log | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 0dedb685-d3a6-4bd6-8afd-58d98853deee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3784d6262bc8f3fd2653b55970d3915587a68276
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346895"
---
# <a name="catalogcleanup_server_log"></a>catalog.cleanup_server_log 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Nettoie les journaux des opérations pour que la base de données SSISDB soit dans un état qui autorise la modification de la valeur de la propriété SERVER_OPERATION_ENCRYPTION_LEVEL.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql
catalog.cleanup_server_log  
```  
  
## <a name="arguments"></a>Arguments  
 Aucun.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) et 1 (échec).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et EXECUTE sur le projet et, si applicable, autorisations READ sur les environnements référencés.  
  
-   L’appartenance au rôle de base de données **ssis_admin**.  
  
-   Appartenance au rôle serveur **sysadmin**.  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 Cette procédure stockée génère des erreurs dans les scénarios suivants :  
  
-   Il existe une ou plusieurs opérations actives dans la base de données SSISDB.  
  
-   La base de données SSISDB n’est pas en mode mono-utilisateur.  
  
## <a name="remarks"></a>Remarques  
 SQL Server 2012 Service Pack 2 a ajouté la propriété SERVER_OPERATION_ENCRYPTION_LEVEL à la table **internal.catalog_properties**. Cette propriété a deux valeurs possibles :  
  
-   **PER_EXECUTION (1)**  : le certificat et la clé symétrique utilisés pour la protection des paramètres d’exécution sensibles et des journaux d’exécution sont créés pour chaque exécution. Vous risquez de rencontrer des problèmes de performances (blocages, échecs de travaux de maintenance, etc.) dans un environnement de production, car les certificats/clés sont générés pour chaque exécution. Toutefois, ce paramètre offre un niveau de sécurité supérieur à l’autre valeur (2).  
  
-   **PER_PROJECT (2)**  : le certificat et la clé symétrique utilisés pour protéger les paramètres sensibles sont créés pour chaque projet. PER_PROJECT (2) est la valeur par défaut. Ce paramètre procure de meilleures performances que le niveau PER_EXECUTION, car la clé et le certificat sont générés une fois par projet et non à chaque exécution.  
  
 Vous devez exécuter la procédure stockée [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md) avant de pouvoir changer SERVER_OPERATION_ENCRYPTION_LEVEL de 2 en 1 ou de 1 en 2. Avant d’exécuter cette procédure stockée, effectuez les opérations suivantes :  
  
1.  Vérifiez que la valeur de la propriété OPERATION_CLEANUP_ENABLED est définie sur TRUE dans la table[catalog.catalog_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
2.  Définissez la base de données Integration Services (SSISDB) sur le mode mono-utilisateur. Dans SQL Server Management Studio, lancez la boîte de dialogue Propriétés de la base de données pour SSISDB, puis, sous l’onglet Options, définissez la propriété Restreindre l’accès sur le mode mono-utilisateur (SINGLE_USER). Après avoir exécuté la procédure stockée cleanup_server_log, définissez la valeur de la propriété sur la valeur d’origine.  
  
3.  Exécutez la procédure stockée [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md).  
  
4.  À présent, changez la valeur de la propriété SERVER_OPERATION_ENCRYPTION_LEVEL dans la table [catalog.catalog_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
5.  Exécutez la procédure stockée [catalog.cleanup_server_execution_keys](../../integration-services/system-stored-procedures/catalog-cleanup-server-execution-keys.md) pour nettoyer les clés et les certificats dans la base de données SSISDB. La suppression des certificats et des clés de la base de données SSISDB pouvant prendre beaucoup de temps, elle doit être exécutée régulièrement pendant les heures creuses.  
  
     Vous pouvez spécifier l’étendue ou le niveau (exécution ou projet) et le nombre de clés à supprimer. La taille de lot par défaut pour la suppression est 1000. Quand vous définissez le niveau sur 2, les clés et les certificats ne sont supprimés que si les projets associés ont été supprimés.  
  
 Pour plus d’informations, consultez l’article suivant de la Base de connaissances : [CORRECTIF : problèmes de performance quand vous utilisez SSISDB comme magasin de déploiement dans SQL Server 2012](https://support.microsoft.com/kb/2972285)  
  
## <a name="example"></a> Exemple  
 L’exemple suivant appelle la procédure stockée cleanup_server_log.  
  
```sql  
USE [SSISDB]  
GO  
  
DECLARE@return_value int  
EXEC@return_value = [internal].[cleanup_server_log]  
SELECT'Return Value' = @return_value  
GO   
```  
  
  
