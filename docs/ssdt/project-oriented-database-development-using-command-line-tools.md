---
title: Développement de base de données orienté projet à l'aide des outils en ligne de commande
description: Affichez les ressources disponibles sur les outils en ligne de commande que SQL Server Data Tools fournit pour travailler avec des fichiers .dacpac, tels que SQLPackage.exe et dbSqlPackage.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9a26def9-8fbd-43e4-9e57-414840b73ed8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: 1439673e40faf8da8cf65da673f0145374b48abd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081890"
---
# <a name="project-oriented-database-development-using-command-line-tools"></a>Développement de base de données orienté projet à l'aide des outils en ligne de commande

SQL Server Data Tools indique quels outils en ligne de commande gèrent des scénarios de développement de base de données orienté projet.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-|-|  
|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)|Cette rubrique décrit l'utilitaire SQLPackage.exe, employé pour les tâches suivantes :<br /><br />- Extraire un fichier .dacpac à partir d’une base de données SQL Server active.<br />- Publier un fichier .dacpac sur une base de données SQL Server active pour procéder à une mise à jour incrémentielle du schéma de la base de données active afin qu’il corresponde au fichier .dacpac.<br />- Comparer un fichier .dacpac à une base de données SQL Server active et générer un script Transact\-SQL de mise à niveau incrémentielle sans mettre à jour la base de données active.<br />- Comparer deux fichiers .dacpac pour générer un script Transact\-SQL de mise à niveau incrémentielle.<br />- Générer un rapport XML qui récapitule les modifications qui se produiraient en cas de mise à niveau incrémentielle de la base de données.|  
|[MSDeploy avec le fournisseur dbSqlPackage](../ssdt/using-msdeploy-with-dbsqlpackage-provider.md)|Cette rubrique décrit le fournisseur de [l’Outil de déploiement Web](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd568996(v=ws.10)) nommé dbSqlPackage inclus avec SSDT, qui fonctionne avec l’Outil de déploiement Web (MSDeploy.exe) Microsoft Internet Information Services (IIS), utilisé pour les tâches suivantes :<br /><br />- Extraire un fichier .dacpac à partir d’une instance SQL Server ou Azure SQL Database locale/distante.<br />- Publier un fichier .dacpac sur une instance SQL Server ou Azure SQL Database distante/locale pour procéder à une mise à niveau incrémentielle de cette dernière.<br />- Publier d’une base de données SQL Server locale vers une instance SQL Server ou Azure SQL Database distante pour procéder à une mise à niveau incrémentielle de la base de données distante.<br />- Comparer un fichier .dacpac à une instance SQL Server ou Azure SQL Database distante/locale pour générer un script Transact\-SQL de mise à niveau incrémentielle sans mettre à jour la base de données active.<br />- Générer un rapport XML qui récapitule les modifications qui se produiraient en cas de mise à niveau incrémentielle de la base de données.|  
  
## <a name="related-sections"></a>Sections connexes  
[Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md)  
