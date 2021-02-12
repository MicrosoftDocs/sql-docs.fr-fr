---
title: Convertir des projets de base de données Visual Studio 2010 en projets de base de données SQL Server
description: Convertissez des projets de base de données Visual Studio 2010 en projets SQL Server et les recibler vers d’autres plateformes. Affichez les objets que SSDT peut et ne peut pas convertir.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.projectconversion.dialog
- sql.data.tools.ImportDAC
ms.assetid: 7e5acf94-5c46-44c7-9ff5-ca7926f5332a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bf8db10c508ed2cdf5fd2f4bc96c0d1d2508d83d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018379"
---
# <a name="how-to-convert-a-visual-studio-2010-database-projects-to-sql-server-database-projects-and-retarget-to-a-different-platform"></a>Procédure : Convertir des projets de base de données Visual Studio 2010 en projets de base de données SQL Server et recibler vers une autre plateforme

Dans SQL Server Data Tools (SSDT), vous pouvez convertir une base de données SQL Server, des objets CLR et des projets d’application de la couche Données existants créés dans Visual Studio 2010 en nouveau projet de base de données SQL Server. Ainsi, vous pouvez tirer parti de la nouvelle expérience de développement de base de données fournie par SSDT, telle que la mise à jour de l'expérience de modification Transact\-SQL, et de la possibilité de recibler votre projet vers Microsoft SQL Server 2012 et SQL Azure avec validation du code. Le processus de conversion convertit les objets (tables, affichages, procédures stockées, fichiers de propriétés ou scripts) qui possèdent un type équivalent dans SSDT, notamment leurs autorisations et fichiers de stratégie de contrôle DAC. Les artefacts qui ne peuvent pas être convertis sont mis en surbrillance dans un journal/rapport de conversion.  
  
Le tableau suivant répertorie tous les artefacts de projet pouvant ou non être convertis par SSDT.  
  
|Artefacts de projet pouvant être convertis|Artefacts de projet ne pouvant pas être convertis|  
|-------------------------------------------|----------------------------------------------|  
|Fichiers projet<br /><br />Fichiers projet .dbproj (base de données et projets serveur Visual Studio 2010, projets d'application de la couche Données)<br /><br />Les fichiers projet CLR .csproj et .vbproj peuvent être convertis, mais peuvent provoquer une perte de données|Projets de tests unitaires de base de données<br /><br />Projets partiels tels que les éléments de fichiers|  
|Fichiers de propriétés<br /><br />Les fichiers *.sqldeployment, .sqlsettings et .sqlpolicy sont convertis en pages de propriétés de projet correspondantes<br /><br />Les fichiers .sqlpermissions sont convertis en scripts Transact\-SQL|Propriétés du projet<br /><br />Server.sqlsettings<br /><br />Variables SQLCMD définies dans des fichiers .sqlcmd|  
|Les fichiers .sql sont importés à l'aide de leur structure de dossiers existante.|Fichiers d'extensibilité|  
|Scripts de prédéploiement et de post-déploiement|Les références de base de données devront être rétablies manuellement après la conversion du projet.|  
|Fichiers de comparaison de schémas|Fichiers de génération de données|  
  
### <a name="to-convert-a-project"></a>Pour convertir un projet  
  
1.  Ouvrez un projet de base de données SQL Server 2005 ou 2008.  
  
2.  L'Assistant **Conversion en projet de base de données SQL Server** s'ouvre automatiquement. Sélectionnez **Convertir en projet de base de données SQL Server**  et cliquez sur **OK**. Conservez le paramètre par défaut pour sauvegarder les fichiers existants extraits.  
  
3.  Un rapport de conversion est automatiquement généré, répertoriant tous les fichiers convertis. Pour consulter des informations supplémentaires sur le processus de conversion, cliquez sur **+** à côté du nom de fichier du projet.  
  
4.  Notez que dans l'**Explorateur de solutions**, le fichier projet, les fichiers de propriétés et les objets de schéma sont tous convertis.  
  
### <a name="to-change-a-projects-target-platform"></a>Pour modifier la plateforme cible d’un projet  
  
1.  Cliquez avec le bouton droit sur le nouveau projet converti dans l'**Explorateur de solutions** et sélectionnez **Propriétés** pour accéder à la boîte de dialogue **Paramètres du projet**.  
  
2.  Dans la liste déroulante **Plateforme cible**, sélectionnez l'une des plateformes prises en charge par SSDT.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : modifier la plateforme cible et publier un projet de base de données](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  
