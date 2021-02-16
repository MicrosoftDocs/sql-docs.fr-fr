---
description: 'Tutoriel : Utiliser la source OData'
title: 'Didacticiel : Utiliser la source OData | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2c64cf8b-5edb-48df-8ffe-697096258f71
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b0bb777c05b30e4b794fc889dc6322e24c4dd5ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339733"
---
# <a name="tutorial-using-the-odata-source"></a>Tutoriel : Utiliser la source OData

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Ce tutoriel vous guide dans le processus d’extraction de la collection **Employees** de l’exemple de service OData **Northwind** (https://services.odata.org/V3/Northwind/Northwind.svc/)), puis de son chargement dans un fichier plat.  
  
## <a name="1-create-an-integration-services-project"></a>1. Créer un projet Integration Services  
  
1.  Lancez **SQL Server Data Tools** ou [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
2.  Cliquez sur **Fichier**, pointez le curseur de la souris sur **Nouveau**, puis cliquez sur **Projet**.  
  
3.  Dans la boîte de dialogue **Nouveau projet** , développez **Installé**, puis **Modèles**, **Business Intelligence**, et cliquez sur **Integration Services**.  
  
4.  Sélectionnez **Projet Integration Services** pour le type de projet.  
  
5.  Entrez un **nom** et sélectionnez un **emplacement** pour le projet, puis cliquez sur **OK**.  
  
## <a name="2-add-and-configure-an-odata-source"></a>2. Ajouter et configurer une source OData 
  
1.  Glissez-déplacez une **Tâche de flux de données** de la **Boîte à outil SSIS** vers l’aire de conception de flux de contrôle pour votre package SSIS.  
  
2.  Cliquez sur l’onglet **Flux de données** ou double-cliquez sur la **tâche de flux de données** pour ouvrir l’aire de conception de flux de données.  
  
3.  Glissez-déplacez la **Source OData** du groupe **Commun** dans la **Boîte à outil SSIS**.
  
4.  Double-cliquez sur le composant **Source OData** pour ouvrir la boîte de dialogue **Éditeur de source OData**.  
  
5.  Cliquez sur **Nouveau...** pour ajouter un nouveau gestionnaire de connexions OData.  
  
6.  Entrez l'URL du service OData pour l' **Emplacement du document de service**. Il peut s’agir de l’URL qui renvoie au document de service, ou bien à un flux ou une entité spécifique. Pour les besoins de ce tutoriel, entrez l’URL vers le document de service : [https://services.odata.org/V3/Northwind/Northwind.svc/](https://services.odata.org/V3/Northwind/Northwind.svc/).  
  
7.  Assurez-vous que l' **Authentification Windows** est sélectionnée pour l' **authentification** à utiliser pour accéder au service OData. L'**Authentification Windows** est sélectionnée par défaut.  
  
8.  Cliquez sur **Tester la connexion** pour tester la connexion, puis cliquez sur **OK** pour créer une instance du gestionnaire de connexions OData.  
  
9. Dans la boîte de dialogue **Éditeur de source OData** , assurez-vous que **Collection** est sélectionné pour l'option **Utiliser la collection sur le chemin d'accès de la ressource** .  
  
10. Dans la liste déroulante **Collection**, sélectionnez **Employés**.  
  
11. Entrez toutes les options ou filtres de requête OData supplémentaires pour **Options de requête**. Par exemple : `$orderby=CompanyName&$top=100`. Pour les besoins de ce didacticiel, entrez `$top=5`.  
  
12. Cliquez sur **Aperçu** pour afficher un aperçu des données.  
  
13. Cliquez sur **Colonnes** dans le volet de navigation gauche pour basculer vers la page **Colonnes** .  
  
14. Sélectionnez **EmployeeID**, **FirstName**, **LastName** et **Colonnes externes disponibles** en activant les cases à cocher.  
  
15. Cliquez sur **OK** pour fermer la boîte de dialogue **Éditeur de source OData** .  
  
## <a name="3-add-and-configure-a-flat-file-destination"></a>3. Ajouter et configurer une destination de fichier plat
  
1.  Glissez-déplacez une **Destination de fichier plat** de la **Boîte à outil SSIS** vers l’aire de conception du flux de données sous le composant **Source OData** .  
  
2.  Connectez le composant **Source OData** au composant **Destination de fichier plat** avec la flèche bleue.  
  
3.  Double-cliquez sur **Destination de fichier plat**. Vous devez voir s'afficher la boîte de dialogue **Éditeur de destination de fichier plat** .  
  
4.  Dans la boîte de dialogue **Éditeur de destination de fichier plat** , cliquez sur **Nouveau** pour créer un nouveau gestionnaire de connexions de fichiers plats.  
  
5.  Dans la boîte de dialogue **Format de fichier plat** , sélectionnez **Délimité**. La boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** s’affiche.  
  
6.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**, entrez `c:\Employees.txt` dans **Nom de fichier**.  
  
7.  Cliquez sur **Colonnes** dans le volet de navigation gauche. Vous pouvez définir les données de cette page.  
  
8.  Cliquez sur OK pour fermer la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** .  
  
9. Dans la boîte de dialogue **Éditeur de destination de fichier plat** , cliquez sur **Mappages** dans le volet de navigation gauche. Vérifiez les mappages.  
  
10. Cliquez sur OK pour fermer la boîte de dialogue **Éditeur de destination de fichier plat** .  

## <a name="4-run-the-package"></a>4. Exécution du package
Exécutez le package SSIS. Vérifiez que le fichier de sortie est créé avec l’ID, le prénom et le nom de cinq employés du flux OData.
  
  
