---
description: Fenêtre de sortie dans SQL Server Management Studio
title: Fenêtre de sortie SSMS
ms.custom: seo-lt-2019
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Output Window [SQL Server Management Studio]
- Activity Monitor [SQL Server Management Studio]
- Object Explorer [SQL Server Management Studio]
ms.assetid: a2ce1a07-b4e2-471c-87d2-b8de5e6c6864
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f123f01811bec35f8418e8e6a5935d581650c261
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353964"
---
# <a name="output-window-in-sql-server-management-studio"></a>Fenêtre de sortie dans SQL Server Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez ouvrir la fenêtre de sortie à partir du menu Affichage ou avec la combinaison de touches Ctrl + Alt + O. Il existe plusieurs canaux de sortie disponibles.

Le tableau suivant donne une vue d’ensemble des types de messages associés à chaque canal de sortie.

|Canal|Description|
|-----------|---------------|  
|**Télémétrie**|La télémétrie est le flux des [données d’utilisation des fonctionnalités anonymes](sql-server-management-studio-ssms.md) collectées par Microsoft. Ces événements peuvent être utiles pour votre archivage des données d’utilisation de SSMS. Elle peut vous permettre d’identifier les nœuds de l’Explorateur d’objets que vous avez développés et les commandes que vous avez exécutées pendant votre session SSMS avec la fenêtre de sortie ouverte.|
|**l’Explorateur d’objets**|Ce canal renvoie le texte de la requête et le temps écoulé des requêtes SQL nécessaires pour développer les nœuds dans l’Explorateur d’objets. Chaque requête journalise une requête Begin et un événement End Query. Chaque événement a un horodatage et l’URN associé à l’entité faisant l’objet d’une requête. L’[URN](/previous-versions/sql/sql-server-2005/ms220608(v=sql.90)) fait référence à l’objet SQL Management Object sous-jacent et se compose d’une hiérarchie de style XPath. Par exemple, l’URN d’une table nommée « Table1 » dans la base de données « Db » sur le serveur « MyServer » est "Server [@Name= 'MyServer']/Database[@Name='Db']/Table[/@Name='Table1']". Le développement d’un nœud dans l’Explorateur d’objets peut effectuer plusieurs requêtes de ce type avec des paramètres différents. L’événement End Query contient le temps écoulé de la requête, ainsi que le texte TSQL. Vous trouverez ces données de requête utiles pour l’analyse des performances de serveur quand l’Explorateur d’objets semble anormalement lent pour développer un nœud spécifique. **Remarque** : Les nœuds de l’Explorateur d’objets ne fournissent pas tous ce niveau de détail de requête lors du développement.|
|**Moniteur d’activité**|Ce canal commence quand le [moniteur d’activité s’ouvre](../relational-databases/performance-monitor/activity-monitor.md) pour un serveur. Ce flux contient des événements indiquant une partie du texte de requête et l’horodatage de chaque requête, les messages d’erreur et les notifications du moniteur en pause en raison de problèmes de connectivité. Si le moniteur d’activité ne semble pas actif ou ne parvient pas à se mettre à jour, vérifiez ce canal de sortie pour plus d’informations.|