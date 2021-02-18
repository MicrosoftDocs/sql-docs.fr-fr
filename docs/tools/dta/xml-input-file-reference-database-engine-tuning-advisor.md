---
title: Référence du fichier d’entrée XML
titleSuffix: Database Engine Tuning Advisor
description: Cet article résume les éléments disponibles pour un fichier d’entrée XML que l’Assistant Paramétrage du moteur de base de données utilise pour paramétrer une base de données.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 05e5e5f0-d6df-4336-b18e-e9bc2835a766
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 184083d89a57c826bb78596a10a4f7e5b61298c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339975"
---
# <a name="xml-input-file-reference-database-engine-tuning-advisor"></a>Référence des fichiers d'entrée XML (Assistant Paramétrage du moteur de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage peut utiliser un fichier d’entrée XML pour paramétrer une base de données. Ce fichier XML désigne les bases de données, les tables, les fichiers ou tables de charge de travail et les options de paramétrage à utiliser pendant la session de paramétrage. Vous pouvez également utiliser ce fichier pour indiquer une configuration spécifiée par l'utilisateur afin d'effectuer une évaluation de simulation.  
  
 Un fichier d’entrée XML de l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] contient une hiérarchie d’éléments XML, chaque élément XML comprenant le texte ou d’autres éléments qui spécifient les paramètres de la session de paramétrage. Le fichier d’entrée XML de l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit être conforme aux normes pour le XML correctement formé. Tous les éléments respectent la casse. Les éléments sont spécifiés à l'aide de la casse Pascal, ce qui signifie que le premier caractère est en majuscules, tout comme la première lettre des mots concaténés suivants.  
  
 Toutes les valeurs d'éléments doivent respecter les conventions d'affectation de noms XML. Pour plus d’informations sur ces conventions, consultez [XML Textual Content](/previous-versions/windows/desktop/ms763742(v=vs.85)) (Contenu textuel XML) dans la bibliothèque MSDN.  
  
 Notez que ce Guide de référence n'est pas complet. Pour plus d'informations sur tous les éléments que vous pouvez utiliser pour définir une entrée XML, reportez-vous au schéma DTASchema.xsd de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="xml-declaration"></a>Déclaration XML  
  
-   [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
## <a name="dtaxml-root-element"></a>Élément racine DTAXML  
  
-   [DTAXML, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/dtaxml-element-dta.md)  
  
## <a name="dtainput-elements"></a>Éléments DTAInput  
  
-   [DTAInput, élément &#40;DTA&#41;](../../tools/dta/dtainput-element-dta.md)  
  
-   [Server, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/server-element-dta.md)  
  
-   [Workload, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/workload-element-dta.md)  
  
-   [Élément TuningOptions &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)  
  
-   [Configuration, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/configuration-element-dta.md)  
  
## <a name="server-elements"></a>Éléments de serveur  
  
-   [Name, élément pour les serveurs &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-server-dta.md)  
  
-   [Database, élément pour les serveurs &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/database-element-for-server-dta.md)  
  
## <a name="workload-elements"></a>Éléments de charge de travail  
  
-   [Élément File &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/file-element-dta.md)  
  
-   [Élément Database &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/database-element-for-workload-dta.md)  
  
-   [Élément EventString &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/eventstring-element-dta.md)  
  
## <a name="tuning-options-elements"></a>Éléments d'options de paramétrage  
  
-   [TuningTimeInMin, élément &#40;DTA&#41;](../../tools/dta/tuningtimeinmin-element-dta.md)  
  
-   [StorageBoundInMB, élément &#40;DTA&#41;](../../tools/dta/storageboundinmb-element-dta.md)  
  
-   [TestServer, élément &#40;DTA&#41;](../../tools/dta/testserver-element-dta.md)  
  
-   [FeatureSet, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/featureset-element-dta.md)  
  
-   [Partitioning, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/partitioning-element-dta.md)  
  
-   [DropOnlyMode, élément &#40;DTA&#41;](../../tools/dta/droponlymode-element-dta.md)  
  
-   [KeepExisting, élément &#40;DTA&#41;](../../tools/dta/keepexisting-element-dta.md)  
  
-   [OnlineIndexOperation, élément &#40;DTA&#41;](../../tools/dta/onlineindexoperation-element-dta.md)  
  
-   [DatabaseToConnect, élément &#40;DTA&#41;](../../tools/dta/databasetoconnect-element-dta.md)  
  
## <a name="configuration-elements"></a>Éléments de configuration  
  
-   [Server, élément pour les configurations &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/server-element-for-configuration-dta.md)  
  
-   [Database, élément pour les configurations &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/database-element-for-configuration-dta.md)  
  
-   [Recommendation, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/recommendation-element-dta.md)  
  
-   [Create, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/create-element-dta.md)  
  
-   [Index, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/index-element-dta.md)  
  
-   [Name, élément pour les index &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-index-dta.md)  
  
-   [Column, élément pour les index &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/column-element-for-index-dta.md)  
  
-   [Name, élément pour les colonnes &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-column-dta.md)  
  
-   [Filegroup, élément pour les index &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/filegroup-element-for-index-dta.md)  
  
## <a name="database-elements"></a>Éléments de base de données  
  
-   [Name, élément pour les bases de données &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-database-dta.md)  
  
-   [Schema, élément pour les bases de données &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/schema-element-for-database-dta.md)  
  
-   [Name, élément pour les schémas &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-schema-dta.md)  
  
-   [Table, élément pour les schémas &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/table-element-for-schema-dta.md)  
  
-   [Name, élément pour les tables &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-table-dta.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)  
  
