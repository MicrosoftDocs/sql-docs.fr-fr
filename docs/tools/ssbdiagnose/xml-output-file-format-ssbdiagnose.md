---
title: Format du fichier de sortie XML
description: Dans SQL Server, l’utilitaire ssbdiagnose peut fournir sa sortie sous forme de fichier XML. Créez une application pour analyser ou signaler des erreurs ou les afficher dans un éditeur XML.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- XML output file format [ssbdiagnose]
- ssbdiagnose
ms.assetid: 3ceb991b-6f15-4504-8828-de5adf448bc5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 7798bc201bdb94a18f4c429e5ca74a04aeceea8a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335895"
---
# <a name="xml-output-file-format-ssbdiagnose"></a>Format du fichier de sortie XML (ssbdiagnose)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La sortie de l’utilitaire **ssbdiagnose** est écrite dans un fichier XML lorsque vous l’exécutez avec le commutateur **-XML** . Le fichier de sortie XML répertorie les informations d'en-tête et les erreurs identifiées dans la configuration [!INCLUDE[ssSB](../../includes/sssb-md.md)] ou la conversation analysée. Vous pouvez écrire une application qui analyse ou effectue un rapport sur les erreurs répertoriées dans le fichier. Vous pouvez également consulter le fichier XML dans tout éditeur XML, comme le bloc-notes XML.  
  
 Un fichier de sortie de **ssbdiangose** contient un élément racine DiagnosticInformation avec deux types enfants :  
  
-   Un élément Banner contenant les informations d'en-tête.  
  
-   Un élément Issue pour chaque problème signalé par **ssbdiagnose**.  
  
## <a name="diagnosticinformation-root-element"></a>Élément racine DiagnosticInformation  
  
-   [Élément DiagnosticInformation &#40;ssbdiagnose&#41;](../../tools/ssbdiagnose/diagnosticinformation-element-ssbdiagnose.md)  
  
## <a name="banner-element"></a>Élément Banner  
  
-   [Élément Banner &#40;ssbdiagnose&#41;](../../tools/ssbdiagnose/banner-element-ssbdiagnose.md)  
  
## <a name="issue-element"></a>Élément Issue  
  
-   [Élément Issue &#40;ssbdiagnose&#41;](../../tools/ssbdiagnose/issue-element-ssbdiagnose.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire ssbdiagnose &#40;Service Broker&#41;](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
  
