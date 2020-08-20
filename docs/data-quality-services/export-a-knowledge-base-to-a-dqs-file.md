---
description: Exporter une base de connaissances dans un fichier .dqs
title: Exporter une base de connaissances dans un fichier .dqs
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: a324ead5-c8aa-4e26-abe3-ef415add00f8
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 946b1192b314c5303cd529c176e93ebf1fb0204f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457925"
---
# <a name="export-a-knowledge-base-to-a-dqs-file"></a>Exporter une base de connaissances dans un fichier .dqs

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Cette rubrique décrit comment exporter une base de connaissances entière dans un fichier de données .dqs dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Vous pouvez exporter un domaine ou la totalité d'une base de connaissances vers un fichier de données. Pour plus d’informations sur l’exportation d’un domaine, consultez [Exporter un domaine vers un fichier .dqs](../data-quality-services/export-a-domain-to-a-dqs-file.md).  
  
 L'exportation d'une base de connaissances vers un fichier .dqs, puis son importation en tant que base de connaissances distincte, simplifie le processus de génération de connaissances, et permet d'économiser aussi bien du temps que des efforts. Cela vous permet de partager une base de connaissances et ses contenus avec d'autres. Le fichier .dqs contient toutes les informations de la base de connaissances, y compris les domaines et la stratégie de correspondance, à l'exception des informations de données de référence jointes. Vous devez joindre à nouveau les domaines requis aux services de données de référence appropriés, le cas échéant, après avoir importé le fichier .dqs. Les données publiées et non publiées dans une base de connaissances sont exportées.  
  
 Le fichier de données .dqs créé par le processus d'exportation est chiffré, de sorte que le contenu ne peut pas être affiché.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Pour exporter une base de connaissances dans un fichier de données .dqs, vous devez avoir créé et ouvert une base de connaissances. Vous n'avez pas besoin de disposer d'un fichier .dqs vers lequel effectuer l'exportation ; il en sera créé un automatiquement.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour exporter une base de connaissances dans un fichier de données .dqs.  
  
##  <a name="export-a-knowledge-base-to-a-dqs-file"></a><a name="Export"></a> Exporter une base de connaissances vers un fichier. DQS  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Exécutez l’Application Data Quality client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Sur l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , ouvrez une base de connaissances dans l'activité Gestion de l'arborescence du domaine.  
  
3.  Dans la page Gestion de l'arborescence du domaine (tout onglet étant sélectionné), cliquez sur l'icône **Exporter des données de Base de connaissances** au-dessus de la liste des domaines, puis sur **Exporter la Base de connaissances**. Vous pouvez également cliquer avec le bouton droit dans la liste **Domaine** , pointer sur **Exporter**, puis cliquer sur **Exporter la Base de connaissances**.  
  
4.  Dans la boîte de dialogue **Exporter vers un fichier de données**, accédez au dossier dans lequel vous voulez enregistrer le fichier, nommez le fichier ou conservez le nom de la base de données, conservez **Fichiers de données DQS (\*.dqs**) comme **Type de fichier**, puis cliquez sur **Enregistrer**.  
  
5.  Dans la boîte de dialogue **Exporter la Base de connaissances** , vérifiez que la ligne d'état indique que l'exportation est terminée. Cliquez sur **OK**.  
  
##  <a name="follow-up-after-exporting-a-domain-to-a-dqs-file"></a><a name="FollowUp"></a> Suivi : après l’exportation d’un domaine vers un fichier. DQS  
 Après avoir exporté une base de connaissances dans un fichier .dqs, vous pouvez importer la base de connaissances dans le même [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] (avec un nouveau nom) ou dans un [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]différent.  
  
  
