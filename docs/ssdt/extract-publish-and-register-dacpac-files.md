---
title: Extraire, publier et enregistrer des fichiers .dacpac.
description: En savoir plus sur les actions que vous pouvez effectuer avec les applications de la couche Données (DAC). Il peut s’agir par exemple de l’extraction, de la publication et de l’inscription de fichiers d’instantanés (.dacpac).
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DacTableChooser
- sql.data.tools.DacPublishDialog
- sql.data.tools.DacPropertiesDialog
- sql.data.tools.publishdacproject
- sql.data.tools.DacExtractDialog
ms.assetid: ed900f93-d3df-40f5-8e62-4d722595e041
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: f7d63d6b26d8f5f07b6a4344e84f3d490012c91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077054"
---
# <a name="extract-publish-and-register-dacpac-files"></a>Extraire, publier et enregistrer des fichiers .dacpac.

Cette rubrique décrit quatre procédures réalisables en cliquant avec le bouton droit sur une base de données connectée dans l’Explorateur d’objets SQL Server :  
  
-   Publier une application de la couche Données  
  
-   Extraire une application de la couche Données  
  
-   Enregistrer une application de la couche Données  
  
-   Annuler l'inscription de l'application de la couche Données  
  
## <a name="publish-data-tier-application"></a>Publier une application de la couche Données  
Publiez un fichier .dacpac dans une base de données. L'action de publication met à jour de manière incrémentielle un schéma de base de données de façon à ce qu'il corresponde au schéma d'un fichier .dacpac source. Si la base de données n'existe pas sur le serveur, elle est créée par l'opération de publication.  
  
Cette action est également disponible lorsque vous sélectionnez le nœud Bases de données.  
  
Sélectionnez le fichier .dacpac. Une fois qu’un fichier .dacpac est sélectionné, le bouton **Propriétés DAC** est activé. La boîte de dialogue **Propriétés DAC** affiche des informations sur le fichier .dacpac.  
  
Spécifiez la chaîne de connexion au serveur de base de données, puis le nom de la base de données, s'il ne figure pas dans la chaîne de connexion.  
  
Les boutons **Publier** et **Générer un script** sont à présent activés. Si vous générez un script, il s'ouvre dans une fenêtre de document et peut être enregistré selon les besoins. Si vous choisissez de publier directement dans la base de données, un résumé de la mise à jour et le script véritablement utilisé s’affichent dans la fenêtre **Opérations Data Tools**.  
  
Si la case **Enregistrer en tant qu’application de la couche Données** est cochée, la base de données obtenue est enregistrée en tant qu’application de la couche Données dans les métadonnées du serveur. Si la base de données dans laquelle vous publiez est inscrite, la publication peut échouer si le schéma de la base de données est différent de celui du fichier .dacpac inscrit.  
  
Des paramètres supplémentaires de configuration de la publication sont disponibles dans la boîte de dialogue **Paramètres de publication avancés**, accessible en cliquant sur le bouton **Avancé**.  
  
## <a name="extract-data-tier-application"></a>Extraire une application de la couche Données  
Extrayez un fichier .dacpac à partir d'une base de données. L’extraction crée un fichier de capture instantanée de base de données (.dacpac) à partir d’une base de données Azure SQL Database ou SQL Server active susceptible de contenir des données provenant de tables d’utilisateur en plus du schéma de la base de données.  
  
Spécifiez le fichier .dacpac à créer. Le bouton **Propriétés DAC** affiche la boîte de dialogue **Propriétés DAC**, qui permet de spécifier les propriétés du fichier .dacpac.  
  
Modifiez la configuration du processus d'extraction selon les besoins. Dans la section **Paramètres d’extraction**, vous pouvez choisir entre extraire uniquement le schéma et extraire le schéma en incluant les données des tables. Si vous choisissez d'extraire le schéma et les données, sélectionnez les tables pour lesquelles vous souhaitez extraire les données.  
  
## <a name="register-data-tier-application"></a>Enregistrer une application de la couche Données  
Enregistrez une base de données en tant qu'application de la couche Données (DAC) dans l'instance. Une représentation de l'état actuel du schéma de la base de données est stocké dans les métadonnées du système.  
  
Dans la boîte de dialogue **Enregistrer une application de la couche Données**, spécifiez les propriétés de l’application DAC enregistrée.  
  
## <a name="unregister-data-tier-application"></a>Annuler l'inscription de l'application de la couche Données  
L'annulation de l'enregistrement vous permet de supprimer les métadonnées d'une application de la couche Données enregistrée de l'instance. Cette opération ne supprime pas la base de données enregistrée.  
  
## <a name="see-also"></a>Voir aussi  
[Développement de base de données connectée](../ssdt/connected-database-development.md)  
  
