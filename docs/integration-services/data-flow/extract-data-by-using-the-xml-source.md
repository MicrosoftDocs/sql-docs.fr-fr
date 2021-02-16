---
description: Extraire des données à l'aide de la source XML
title: Extraire des données à l’aide de la source XML | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- extracting data [Integration Services]
- sources [Integration Services], XML
- XML source [Integration Services]
ms.assetid: 5d5be54c-2b7e-4957-9193-c5ea5c5d6d15
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 462ec77dbb8faf87fd8cb53806e3b8169b74317b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339759"
---
# <a name="extract-data-by-using-the-xml-source"></a>Extraire des données à l'aide de la source XML

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Pour pouvoir ajouter et configurer une source XML, le package doit inclure au moins une tâche de flux de données.  
  
### <a name="to-extract-data-using-an-xml-source"></a>Pour extraire des données à l'aide d'une source XML  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** puis, dans la **Boîte à outils**, faites glisser la source XML vers l’aire de conception.  
  
4.  Double-cliquez sur la source XML.  
  
5.  Dans **Éditeur de source XML**, dans la page **Gestionnaire de connexions** , sélectionnez un mode d’accès aux données :  
  
    -   Pour le mode d’accès **Emplacement du fichier XML** , cliquez sur **Parcourir** et recherchez le dossier qui contient le fichier XML.  
  
    -   Pour le mode d’accès **Fichier XML à partir d’une variable** , sélectionnez la variable définie par l’utilisateur qui contient le chemin du fichier XML.  
  
    -   Pour le mode d’accès **Données XML à partir d’une variable** , sélectionnez la variable définie par l’utilisateur qui contient les données XML.  
  
    > [!NOTE]  
    >  Les variables doivent être définies dans la même étendue que la tâche de flux de données qui contient la source XML ou dans la même étendue que le package. Par ailleurs, les données de la variable doivent être de type string.  
  
6.  Si vous le souhaitez, sélectionnez **Utiliser le schéma inclus** pour indiquer que le document XML inclut des informations de schéma.  
  
7.  Pour spécifier une schéma XSD (XML Schema definition language) externe pour le fichier XML, procédez de l'une des manières suivantes :  
  
    -   Cliquez sur **Parcourir** pour rechercher un fichier XSD existant.  
  
    -   Cliquez sur **Créer XSD** pour créer un fichier XSD à partir du fichier XML.  
  
8.  Pour mettre à jour le nom des colonnes de sortie, cliquez sur **Colonnes** et modifiez les valeurs dans la liste **Colonne de sortie** .  
  
9. Pour configurer l'affichage des erreurs, cliquez sur **Sortie d'erreur**. Pour plus d’informations, consultez [Débogage d’un flux de données](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
10. Cliquez sur **OK**.  
  
11. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a>Voir aussi  
 [Source XML](../../integration-services/data-flow/xml-source.md)   
 [Transformations Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Chemins Integration Services](../../integration-services/data-flow/integration-services-paths.md)   
 [tâche de flux de données](../../integration-services/control-flow/data-flow-task.md)  
  
  
