---
title: Rechercher la version du schéma de définition de rapport | Microsoft Docs
description: Découvrez comment identifier la version du schéma RDL de votre fichier de définition de rapport.
ms.date: 06/06/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- XML schemas [Reporting Services]
- Report Definition Language, XML schema
- schemas [Reporting Services]
ms.assetid: 67954419-1b61-4481-a3b9-23b4ba7a5624
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f3e656d797f876fdd40ab1904f153a332673f504
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069454"
---
# <a name="find-the-report-definition-schema-version-ssrs"></a>Rechercher la version du schéma de définition de rapport (SSRS)

Un fichier de définition de rapport spécifie l'espace de noms RDL de la version du schéma de définition de rapport qui est utilisée pour valider le fichier rdl. Lorsque vous ouvrez un fichier .rdl dans un environnement de création de rapports comme le Concepteur de rapports dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], Visual Studio ou le Générateur de rapports. Si le rapport a été créé pour un espace de noms précédent, un fichier de sauvegarde est automatiquement créé et le rapport est mis à niveau vers l’espace de noms actuel. Si vous enregistrez la définition de rapport mise à niveau, vous enregistrez le fichier .rdl converti. Il s'agit de la seule façon de mettre à niveau une définition de rapport. La définition de rapport proprement dite n'est pas mise à niveau sur un serveur de rapports. Le rapport compilé est mis à niveau sur un serveur de rapports. Pour plus d'informations, consultez [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md).  
  
## <a name="how-to-identify-the-rdl-schema-version-of-a-report"></a>Procédure : Identifier la version du schéma RDL d’un rapport  
  
1. Ouvrez le fichier de rapport .rdl dans une application, telle que le Bloc-notes ou XML Notepad, dans laquelle vous pouvez visualiser le fichier XML.  
  
     L'élément de rapport XML indique l'espace de noms du schéma. Par exemple, l'élément de rapport suivant indique l'espace de noms pour le Concepteur de rapports et l'espace de noms pour la définition du rapport.  
  
    ``` XML 
    <Report xmlns:rd="http://schemas.microsoft.com/SQLServer/reporting/reportdesigner" xmlns="http://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition" xmlns:df="http://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition/defaultfontfamily" MustUnderstand="df">  
    ```  
  
     L’espace de noms de la définition de rapport le plus récent est 2016. Toutefois, l’espace de noms de définition de rapport publié le plus récent est 2010, spécifié par l’URL suivante : `https://schemas.microsoft.com/sqlserver/reporting/2010/01/reportdefinition`.
  
### <a name="how-to-identify-the-rdl-schema-version-of-report-designer"></a>Procédure : Identifier la version du schéma RDL du Concepteur de rapports  
  
1.  Ouvrez un nouveau projet. La version du projet que vous choisissez détermine la version du schéma RDL. Dans SQL Server, plusieurs versions de schéma sont prises en charge. Pour plus d’informations, consultez [Déploiement et prise en charge des versions dans SQL Server Data Tools](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
2.  Dans le menu **Projet** , cliquez sur **Ajouter un nouvel élément**. La boîte de dialogue **Ajouter un nouvel élément** s’ouvre.  
  
3.  Dans le volet **Modèles** , cliquez sur **Rapport**.  
  
4.  Dans la zone **Nom**, tapez un nom de rapport ou acceptez la valeur par défaut.  
  
5.  Cliquez sur **Add**. Le Concepteur de rapports ouvre un rapport vide en mode Création.  
  
6.  Dans le menu **Affichage** , cliquez sur **Code**. La définition du rapport s'affiche sous forme de fichier XML.  
  
    L'élément de rapport XML indique l'espace de noms du schéma. Par exemple, l'élément de rapport suivant indique l'espace de noms pour le Concepteur de rapports et l'espace de noms pour la définition du rapport.  
  
    ``` XML 
    <Report xmlns:rd="http://schemas.microsoft.com/SQLServer/reporting/reportdesigner" xmlns="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition" xmlns:df="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition/defaultfontfamily" MustUnderstand="df">  
    ```  
  
     L'espace de noms de la définition de rapport est spécifié par l'URL suivante : `https://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition`  
  
### <a name="how-to-identify-the-rdl-schema-version-on-the-report-server"></a>Procédure : Identifier la version du schéma RDL sur le serveur de rapports  
  
-   Dans le portail web, tapez l'URL du serveur de rapports. Par exemple, l'URL suivante spécifie un serveur de rapports sur l'ordinateur local :  
  
     `https://localhost/reportserver/reportdefinition.xsd`  
  
     Le fichier .xsd s'ouvre dans le navigateur.  
  
     L'élément de schéma XML indique l'espace de noms du schéma. Par exemple, l’élément de schéma suivant indique trois espaces de noms : la référence targetNamespace utilisée en interne par [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], la référence xsd pour le schéma lui-même (xsd) et la référence de définition de rapport.  *Year* représente l’année du schéma que le rapport utilise. Par exemple, 2010 ou 2016.
  
    ``` XML  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition" targetNamespace="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition" elementFormDefault="qualified">  
    ```  
  
     L'espace de noms de la définition de rapport est spécifié par l'URL suivante : `https://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition`  

## <a name="next-steps"></a>Étapes suivantes
[Rapports de mise à niveau](../../reporting-services/install-windows/upgrade-reports.md)   
[Langage de définition des rapports](../../reporting-services/reports/report-definition-language-ssrs.md)   

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
