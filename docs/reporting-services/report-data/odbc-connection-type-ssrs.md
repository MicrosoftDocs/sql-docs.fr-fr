---
title: Type de connexion ODBC | Microsoft Docs
description: Utilisez les informations de cet article sur le type de connexion ODBC pour savoir comment créer une source de données.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 24163866-f37a-4c38-982e-c3d79bf64d4c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8e41696695ec8a95dd16d1f30724147cdd359a1
ms.sourcegitcommit: 9470c4d1fc8d2d9d08525c4f811282999d765e6e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2020
ms.locfileid: "86458971"
---
# <a name="odbc-connection-type-ssrs"></a>Type de connexion ODBC (SSRS)
  Pour inclure les données d'un fournisseur de données ODBC, vous devez avoir un dataset basé sur une source de données de rapport de type ODBC. Ce type de source de données intégré est basé sur l’extension pour le traitement des données ODBC [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Chaîne de connexion  
 La chaîne de connexion de l'extension pour le traitement des données ODBC dépend du pilote ODBC souhaité. Une chaîne de connexion classique contient des paires nom/valeur prises en charge par le pilote. Par exemple, la chaîne de connexion suivante spécifie le pilote ODBC pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et la base de données AdventureWorks :  
  
```  
Driver={SQL Server Native Client 10.0};Server=server;Database=AdventureWorks;Trusted_Connection=yes;  
```  
  
  
##  <a name="credentials"></a><a name="Credentials"></a> Informations d'identification  
 Les informations d'identification sont obligatoires pour exécuter des requêtes, afficher l'aperçu du rapport localement et afficher l'aperçu du rapport à partir du serveur de rapports.  
  
 Après avoir publié votre rapport, vous pouvez devoir modifier les informations d'identification pour la source de données afin que les autorisations soient valides pour récupérer les données lorsque le rapport s'exécute sur le serveur de rapports.  
  
 Si vous configurez votre source de données ODBC de manière à demander un mot de passe ou à inclure le mot de passe dans la chaîne de connexion, et si l'utilisateur entre le mot de passe avec des caractères spéciaux tels que des marques de ponctuation, certains pilotes de sources de données sous-jacents ne peuvent pas valider les caractères spéciaux. Lors du traitement du rapport, le message « Mot de passe non valide » peut s'afficher et signaler ce problème. Si le changement du mot de passe s'avère impossible, vous pouvez demander à votre administrateur de base de données de stocker les informations d'identification appropriées sur le serveur de rapports en tant que nom de source de données (DSN) ODBC. Pour plus d'informations, consultez « OdbcConnection.ConnectionString » dans la documentation du Kit de développement logiciel (SDK) [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] .  
  
> [!NOTE]  
>  Il est recommandé de ne pas ajouter d'informations de connexion, notamment des mots de passe, à la chaîne de connexion. Le Générateur de rapports comprend un onglet distinct dans la boîte de dialogue **Source de données** , où vous pouvez entrer les informations d'identification.  
  
 Pour plus d’informations, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) ou [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Notes  
 ODBC est une ancienne technologie d'accès aux données qui a précédé OLEDB. ODBC prend en charge uniquement les sources de données relationnelles. Les fournisseurs de données ODBC sont appelés des *pilotes*. Les pilotes ODBC sont fournis par Microsoft et des éditeurs tiers. Par exemple, Microsoft Office installe des pilotes ODBC qui se connectent aux formats de fichiers Office.  
  
 Avant de pouvoir créer une chaîne de connexion ODBC, vous devez disposer de pilotes ODBC installés et créer un nom de source de données d'ordinateur ou système. Pour récupérer correctement les données de votre choix, vous devez spécifier une syntaxe de requête prise en charge par le pilote. La prise en charge des paramètres varie selon le pilote. Pour plus d’informations, consultez les rubriques spécifiques au pilote que vous sélectionnez, par exemple [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md).  
  
###### <a name="platform-and-version-information"></a>Informations sur les plateformes et les versions  
 Pour plus d’informations sur les fournisseurs de données ODBC spécifiques, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).
  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Rubriques de procédures  
 Cette section contient des instructions pas à pas sur l'utilisation des connexions de données, des sources de données et des datasets.  
  
 [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Ajouter un filtre à un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="related-sections"></a><a name="Related"></a> Sections connexes  
 Ces sections de la documentation fournissent des informations de fond d'ordre conceptuel sur les données de rapport, ainsi que des informations sur les procédures de définition, de personnalisation et d'utilisation des parties d'un rapport qui sont liées aux données.  
  
 [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
 Fournit une vue d'ensemble de l'accès aux données pour votre rapport.  
  
 [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 Fournit des informations sur les connexions de données et les sources de données.  
  
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 Fournit des informations sur les datasets incorporés et partagés.  
  
 [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 Fournit des informations sur la collection de champs de dataset générée par la requête.  
  
 [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md). Fournit des informations détaillées sur la prise en charge des plateformes et des versions pour chaque extension de données.  
  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  
