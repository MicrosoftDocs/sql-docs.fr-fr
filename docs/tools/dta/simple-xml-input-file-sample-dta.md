---
title: Exemple de fichier d'entrée XML simple (DTA)
description: Cet article contient un exemple de fichier d’entrée XML à utiliser pour le paramétrage des charges de travail avec l’Assistant Paramétrage du moteur de base de données.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- sample applications [DTA]
ms.assetid: 5b00e4eb-1742-43ec-98d8-d84216b6b840
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 1b475ce68b38283903ecd7295a89b18fce6b9636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354413"
---
# <a name="simple-xml-input-file-sample-dta"></a>Exemple de fichier d'entrée XML simple (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Copiez et collez cet exemple de fichier d'entrée XML simple à utiliser pour le paramétrage des charges de travail dans votre éditeur XML ou de texte favori. Remplacez ensuite les valeurs spécifiées pour les éléments **Server**, **Database**, **Schema**, **Table**, **Workload** et **TuningOptions** par celles dont vous avez besoin pour votre session de paramétrage. Pour plus d’informations sur les attributs et les éléments enfants que vous pouvez utiliser avec ces éléments, consultez la [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md). L'exemple ci-dessous utilise uniquement un sous-ensemble des options d'attributs et d'éléments enfants disponibles.  
  
## <a name="code"></a>Code  
 [!code-xml[InputFileSamples#SimpleXMLInputFile](../../tools/dta/codesnippet/xml/simple-xml-input-file-sa_1.xml)]  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
