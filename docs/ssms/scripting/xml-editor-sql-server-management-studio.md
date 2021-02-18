---
title: Éditeur XML (SQL Server Management Studio)
description: Découvrez comment utiliser les outils visuels de l’éditeur XML de SQL Server Management Studio pour travailler avec des schémas XML (XSD), des jeux de données ADO.NET et des documents XML.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.editor.xml.f1
- sql13.swb.editorxml.f1
- vs.xmleditor
- sql13.swb.xmleditor.f1
helpviewer_keywords:
- XML Designer [SQL Server Management Studio]
ms.assetid: 0824a5ce-e67b-4b53-98d9-d371faf2d23c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24a4fa3e7957ca7e2d47188a7f3f982f3b8cff59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346089"
---
# <a name="xml-editor-sql-server-management-studio"></a>Éditeur XML (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Fournit un ensemble d'outils visuels pour utiliser des schémas XML, des groupes de données ADO.NET et des documents XML. Le Concepteur XML prend en charge le langage de définition de schéma XML (XSD, XML Schema Definition) défini par le World Wide Web Consortium (WC3), mais pas les définitions de type de document (DTD) ni les autres langages de schéma XML, tel XDR (XML-Data Reduced).  

Pour afficher le concepteur, ajoutez un groupe de données, un schéma XML ou un fichier XML à votre projet ou ouvrez l'un des types de fichiers énumérés dans le tableau suivant.  

> [!CAUTION]
> La commande **Annuler** n'est pas disponible lorsque vous travaillez en mode Schéma. Planifiez soigneusement votre travail et enregistrez souvent vos fichiers.  
  
 Le concepteur propose trois vues (ou modes) pour travailler sur les fichiers XML, les schémas XML et les groupes de données :  
  
|Affichage|Description|Types de fichiers pris en charge|  
|----------|-----------------|--------------------------|  
|**Schéma**|Pour créer et modifier visuellement des schémas XML et des groupes de données ADO.NET.|.xsd|  
|**Données**|Pour modifier visuellement des fichiers de données XML dans une grille de données structurée.|.xml|  
|**XML**|Pour modifier XML ; l'éditeur de code source fournit un codage en couleurs et IntelliSense, y compris Compléter le mot et Liste des membres.|.xml .xsd .xslt .wsdl.web.resx.tdl.wsf.hta.disco.vsdisco.config|  
|**ShowPlan**|Affiche les plans de requête XML créés à l'aide de l'option SET SHOWPLAN_XML ON.|.showplan|  
  
## <a name="schema-view"></a>Mode Schéma  
 Le mode Schéma fournit une représentation visuelle des éléments, attributs, types, etc., qui constituent les schémas XML et les groupes de données ADO.NET.  
  
 Le mode Schéma vous permet de construire des schémas et des groupes de données en déplaçant des éléments sur l'aire de conception soit à partir de l'onglet Schéma XML de la Boîte à outils, soit à partir de l'Explorateur de serveurs. Vous pouvez également ajouter des éléments au concepteur en cliquant avec le bouton droit sur l'aire de conception et en sélectionnant Ajouter dans le menu contextuel.  
  
 En mode Schéma, vous pouvez :  
  
-   construire et modifier des schémas XML et des groupes de données ADO.NET ;  
  
-   créer et modifier des relations entre les tables ;  
  
-   créer et modifier des clés ;  
  
-   générer des groupes de données ADO.NET à partir de schémas XML.  
  
> [!NOTE]  
>  La disposition des éléments en mode Schéma est stockée dans le fichier .xsx, que vous pouvez afficher en cliquant sur **Afficher tous les fichiers** dans la barre d'outils de l'Explorateur de solutions, puis en développant le fichier .xsd. Si aucun fichier .xsx n'est présent, cela signifie que le fichier .xsd n'a jamais été ouvert dans le Concepteur XML.  
  
### <a name="customizing-schema-view"></a>Personnalisation du mode Schéma  
 Les fonctionnalités suivantes permettent de modifier la présentation visuelle des éléments en mode Schéma :  
  
-   zoom ;  
  
-   développement et réduction des éléments imbriqués ;  
  
-   réorganisation automatique de la disposition des éléments ;  
  
-   rétablissement de l'état par défaut des éléments réduits.  
  
##### <a name="to-expand-hidden-nested-elements"></a>Pour développer les éléments imbriqués masqués  
  
-   Cliquez sur l'icône plus (+) en bas de l'élément.  
  
##### <a name="to-collapse-nested-elements"></a>Pour réduire les éléments imbriqués  
  
-   Cliquez sur l'icône moins (-) de l'élément le plus bas que vous souhaitez afficher dans le concepteur.  
  
## <a name="data-view"></a>Vue de données  
 Le mode Données affiche une grille de données dont vous pouvez vous servir pour modifier les fichiers .xml. Seul le contenu d'un fichier XML (pas les balises ni la structure) peut être modifié en mode Données.  
  
 Il y a deux zones séparées dans la Vue de données : **Tables de données** et **Données**. La zone **Tables de données** est une liste des relations définies à l’intérieur du fichier XML, dans l’ordre de leur imbrication (en allant de l’extérieur vers l’intérieur). La zone **Données** est une grille de données qui affiche les données en fonction de la sélection effectuée dans la zone Tables de données.  
  
> [!NOTE]  
>  Les fichiers XML nouvellement créés ne contiennent pas de données et ne peuvent donc pas être affichés en mode Données. Il existe également certaines instances de documents XML où il est impossible d'appeler le mode Données. Même si le document XML est considéré comme correct, si les données essayant de passer en mode Données ne sont pas structurées, le message suivant s'affiche : « Bien que ce document soit correctement construit, il contient une structure impossible à afficher en mode Données. »  
  
 En mode Données, vous pouvez :  
  
-   remplir les tables de données manuellement ;  
  
-   modifier les tables de données existantes ;  
  
-   générer un schéma XML à partir d'un document XML.  
  
## <a name="xml-view"></a>Mode XML  
 Le mode XML fournit un éditeur pour modifier le XML brut, IntelliSense et un codage en couleurs. La saisie semi-automatique des instructions est disponible lorsque vous travaillez sur des fichiers .xsd et .xml auxquels est associé un schéma. Tapez < pour commencer une balise : la liste des éléments valides à cet emplacement s'affiche. Après avoir tapé le nom de l'élément et appuyé sur la barre d'espace, la liste des attributs pris en charge par l'élément s'affiche.  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] IntelliSense ne sont pas disponibles sur la barre d'outils. Pour accéder à ces options lorsque vous êtes dans l'Éditeur XML, dans le menu **Edition** , cliquez sur **IntelliSense**.  
  
## <a name="showplan-view"></a>Mode SHOWPLAN  
 Les plans de requête peuvent être enregistrés au format XML lorsqu'ils sont créés à l'aide de l'option SET SHOWPLAN_XML ON. Double-cliquez sur un fichier se terminant par l'extension .showplan pour ouvrir le plan de requête.  
  
## <a name="see-also"></a>Voir aussi  
 [Enregistrer un plan d’exécution au format XML](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
