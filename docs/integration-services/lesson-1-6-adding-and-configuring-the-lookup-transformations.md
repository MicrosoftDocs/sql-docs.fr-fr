---
description: 'Leçon 1-6 : Ajouter et configurer les transformations de recherche'
title: 'Étape 6 : Ajouter et configurer les transformations de recherche | Microsoft Docs'
ms.custom: ''
ms.date: 03/19/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c59f723-9707-4407-80ae-f05f483cf65f
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.openlocfilehash: dbfe5a2fd1472ce67edddeb2eff8a223352dccce
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125611"
---
# <a name="lesson-1-6-add-and-configure-the-lookup-transformations"></a>Leçon 1-6 : Ajouter et configurer les transformations de recherche

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Une fois que vous avez configuré la source de fichier plat pour extraire les données du fichier source, vous définissez les transformations de recherche nécessaires pour obtenir les valeurs de **CurrencyKey** et **DateKey**. Une transformation de recherche effectue une recherche en joignant les données dans la colonne d'entrée spécifiée à une colonne dans un dataset de référence. Le dataset de référence peut être une table ou une vue existante, une nouvelle table ou le résultat d'une instruction SQL. Dans ce tutoriel, la transformation de recherche utilise un gestionnaire de connexions OLE DB pour se connecter à la base de données qui contient les données sources du jeu de données de référence.  
  
> [!NOTE]  
> Vous pouvez également configurer la transformation de recherche afin qu'elle se connecte à un cache qui contient le jeu de données de référence. Pour plus d’informations, consultez [Transformation de recherche](../integration-services/data-flow/transformations/lookup-transformation.md).  
  
Au cours de cette tâche, vous allez ajouter et configurer les deux composants de transformation de recherche suivants dans le package :  
  
-   Une transformation qui effectue une recherche des valeurs dans la colonne **CurrencyKey** de la table de dimensions **DimCurrency**, en fonction de leur correspondance avec les valeurs de la colonne **CurrencyID** du fichier plat.  
  
-   Une transformation qui effectue une recherche des valeurs dans la colonne **DateKey** de la table de dimensions **DimDate**, en fonction de leur correspondance avec les valeurs de la colonne **CurrencyDate** du fichier plat.  
  
Dans les deux cas, la transformation de recherche utilise le gestionnaire de connexions OLE DB que vous avez créé précédemment.  
  
## <a name="add-and-configure-the-lookup-currency-key-transformation"></a>Ajouter et configurer la transformation Lookup Currency Key  
  
1.  Dans la **boîte à outils SSIS**, développez **Commun**, puis faites glisser **Recherche** sur l'aire de conception de l'onglet **Flux de données** . Placez la **recherche** directement sous la source **Extract Sample Currency Data**.  
  
2.  Sélectionnez la source de fichier plat **Extract Sample Currency Data** et faites glisser sa flèche bleue vers la transformation de **recherche** que vous venez d’ajouter pour connecter les deux composants.  
  
3.  Dans l’aire de conception **Flux de données**, sélectionnez **Recherche** dans la transformation de **recherche**, puis remplacez le nom par **Lookup Currency Key**.  
  
4.  Double-cliquez sur la transformation **Lookup Currency Key** pour afficher l’**éditeur de transformation de recherche**.  
  
5.  Dans la page **Général** , effectuez les sélections suivantes :  
  
    1.  Sélectionnez **Cache complet**.  
  
    2.  Dans la zone **Type de connexion** , sélectionnez **Gestionnaire de connexions OLE DB**.  
  
6.  Dans la page **Connexion** , effectuez les sélections suivantes :  
  
    1.  Dans la boîte de dialogue **Gestionnaire de connexions OLE DB** , assurez-vous que **localhost.AdventureWorksDW2012** est affiché.  
  
    2.  Sélectionnez **Utiliser les résultats d’une requête SQL**, puis entrez ou collez l’instruction SQL suivante :  
  
        ```sql
        SELECT * FROM [Sales].[Currency]
        WHERE [CurrencyCode]
        IN ('ARS', 'AUD', 'BRL', 'CAD', 'CNY',
            'DEM', 'EUR', 'FRF', 'GBP', 'JPY',
            'MXN', 'SAR', 'USD', 'VEB')
        ```  
    3.  Sélectionnez **Aperçu** pour vérifier les résultats de la requête.
  
7.  Dans la page **Colonnes** , effectuez les sélections suivantes :  
  
    1.  Dans le volet **Colonnes d'entrée disponibles** , faites glisser **CurrencyID** vers le volet **Colonnes de recherche disponibles** et déposez cet élément sur **CurrencyAlternateKey**.  
  
    2.  Dans la liste **Colonnes de recherche disponibles** , activez la case à cocher située à gauche de **CurrencyKey**.  
  
8.  Sélectionnez **OK** pour revenir à l’aire de conception **Flux de données**.  
  
9. Cliquez avec le bouton droit sur la transformation Lookup Currency Key, puis sélectionnez **Propriétés**.  
  
10. Dans la fenêtre **Propriétés**, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)** et la propriété **DefaultCodePage** la valeur **1252**.  
  
## <a name="add-and-configure-the-lookup-date-key-transformation"></a>Ajouter et configurer la transformation Lookup Date Key  
  
1.  Dans la **Boîte à outils SSIS**, faites glisser **Recherche** vers l'aire de conception **Flux de données** . Placez cette **recherche** directement sous la transformation **Lookup Currency Key**.  
  
2.  Sélectionnez la transformation **Lookup Currency Key** et faites glisser sa flèche bleue vers la nouvelle transformation de **recherche** pour connecter les deux composants.  
  
3.  Dans la boîte de dialogue **Sélection entrée et sortie**, sélectionnez **Sortie de recherche avec correspondance** dans la zone de liste de **Sortie**, puis sélectionnez **OK**.  
  
4.  Dans l’aire de conception **Flux de données**, sélectionnez le nom **Recherche** dans la transformation de **recherche** que vous venez d’ajouter, puis remplacez ce nom par **Lookup Date Key**.  
  
5.  Double-cliquez sur la transformation **Lookup Date Key** .  
  
6.  Dans la page **Général** , sélectionnez **Cache partiel**.  
  
7.  Dans la page **Connexion** , effectuez les sélections suivantes :  
  
    1.  Dans la boîte de dialogue **Gestionnaire de connexions OLE DB**, vérifiez que **localhost.AdventureWorksDW2012** est affiché.  
  
    2.  Dans la zone **Utiliser une table ou une vue**, entrez ou sélectionnez **[dbo].[DimDate]**.  
  
8.  Dans la page **Colonnes** , effectuez les sélections suivantes :  
  
    1.  Dans le volet **Colonnes d'entrée disponibles** , faites glisser **CurrencyDate** vers le volet **Colonnes de recherche disponibles** et déposez cet élément sur **FullDateAlternateKey**.  Si vous voyez un message indiquant une incompatibilité de type de données, changez le type de données de CurrencyDate en [DT_DBDATE].
  
    2.  Dans la liste **Colonnes de recherche disponibles** , activez la case à cocher située à gauche de **DateKey**.  
  
9. Dans la page **Avancé** , examinez les options de mise en cache.  
  
10. Sélectionnez **OK** pour revenir à l’aire de conception **Flux de données**.  
  
11. Cliquez avec le bouton droit sur la transformation **Lookup Date Key**, puis sélectionnez **Propriétés**.
  
12. Dans la fenêtre **Propriétés**, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)** et la propriété **DefaultCodePage** la valeur **1252**.  
  
## <a name="go-to-next-task"></a>Passer à la tâche suivante
[Étape 7 : Ajouter et configurer la destination OLE DB](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
## <a name="see-also"></a>Voir aussi  
[Transformation de recherche](../integration-services/data-flow/transformations/lookup-transformation.md)  
