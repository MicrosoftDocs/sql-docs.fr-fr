---
description: catalog.deploy_project (base de données SSISDB)
title: catalog.deploy_project (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2e3439b4-7226-4b61-a993-7a1d161eac7e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ef918ad60dd685cf90fc043eba72dcc6dd7987ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342249"
---
# <a name="catalogdeploy_project-ssisdb-database"></a>catalog.deploy_project (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Déploie un projet dans un dossier dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ou met à jour un projet existant qui a été déployé précédemment.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.deploy_project [@folder_name =] folder_name   
      , [ @project_name = ] project_name   
      , [ @project_stream = ] projectstream   
    [ , [ @operation_id = ] operation_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Arguments  
 [@folder_name =] *folder_name*  
 Nom du dossier où le projet est déployé. *folder_name* est de type **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Nom du nouveau projet ou du projet mis à jour dans le dossier. *project_name* est de type **nvarchar(128)** .  
  
 [@projectstream =] *projectstream*  
 Contenu binaire du fichier de déploiement d'un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (extension .ispac).  
  
 Vous pouvez utiliser une instruction SELECT avec la fonction OPENROWSET et le fournisseur d'ensembles de lignes BULK pour récupérer le contenu binaire du fichier. Pour obtenir un exemple, consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md). Pour plus d’informations sur la clause OPENROWSET, consultez [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
 *projectstream* est de type **varbinary(MAX)**  
  
 [@operation_id =] *operation_id*  
 Retourne l'identificateur unique de l'opération de déploiement. *operation_id* est de type **bigint**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations CREATE_OBJECTS sur le dossier pour déployer un nouveau projet ou des autorisations MODIFY sur le projet pour mettre à jour un projet  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur de cette procédure stockée :  
  
-   Un paramètre fait référence à un objet qui n'existe pas, un paramètre essaie de créer un objet qui existe déjà, ou un paramètre n'est pas valide d'une autre manière  
  
-   La valeur du paramètre *\@project_name* ne correspond pas au nom du projet dans le fichier de déploiement  
  
-   L'utilisateur n'a pas des autorisations suffisantes.  
  
## <a name="remarks"></a>Notes  
 Pendant un déploiement ou une mise à jour de projet, la procédure stockée ne vérifie pas le niveau de protection des packages individuels dans le projet.  
  
  
