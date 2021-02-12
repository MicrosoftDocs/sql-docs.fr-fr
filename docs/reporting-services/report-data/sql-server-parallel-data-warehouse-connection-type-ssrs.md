---
title: Type de connexion à un entrepôt de données SQL Server Parallel Data Warehouse | Microsoft Docs
description: Utilisez les informations de cet article sur le type de connexion SQL Server Parallel Data Warehouse pour savoir comment créer une source de données.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 3925fd3d-2aa1-4768-96ad-cfc2c0ba9283
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dfa373b98dfda2af0c8d18b1cc58fc7f0df5a1cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030981"
---
# <a name="sql-server-parallel-data-warehouse-connection-type-ssrs"></a>Type de connexion à un entrepôt de données SQL Server Parallel Data Warehouse (SSRS)

  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDWCurrentFull](../../includes/ssdwcurrentfull-md.md)] est un appareil d'entrepôt de données évolutif qui offre performances et montée en charge via le traitement parallèle massif. [!INCLUDE[ssDW](../../includes/ssdw-md.md)] utilise des bases de données SQL Server pour le traitement distribué et le stockage des données.  
  
 L’appareil partitionne les grandes tables de base de données entre plusieurs nœuds physiques, chaque nœud exécutant sa propre instance de SQL Server. Lorsqu'un rapport se connecte à [!INCLUDE[ssDW](../../includes/ssdw-md.md)] pour récupérer les données de rapport, il se connecte au nœud de contrôle, qui gère le traitement des requête dans l'appareil [!INCLUDE[ssDW](../../includes/ssdw-md.md)] . Après avoir établi la connexion, il n'y a pas de différences entre utiliser une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est ou n'est pas dans un environnement [!INCLUDE[ssDW](../../includes/ssdw-md.md)] .  
  
 Pour inclure des données de [!INCLUDE[ssDW](../../includes/ssdw-md.md)] dans votre rapport, vous devez avoir un dataset basé sur une source de données de rapport de type [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Parallel Data Warehouse. Ce type de source de données intégré est basé sur l’extension de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Parallel Data Warehouse. Utilisez ce type de source de données pour vous connecter à et récupérer des données de [!INCLUDE[ssDW](../../includes/ssdw-md.md)].  
  
 Cette extension de données prend en charge des paramètres à valeurs multiples, agrégats de serveur et informations d'identification gérés séparément de la chaîne de connexion.  
   
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Chaîne de connexion  
 Lorsque vous vous connectez à [!INCLUDE[ssDW](../../includes/ssdw-md.md)], vous vous connectez à un objet de base de données dans une appliance [!INCLUDE[ssDW](../../includes/ssdw-md.md)] . Vous spécifiez l'objet de base de données à utiliser dans le concepteur de requêtes. Si vous ne spécifiez pas de base de données dans la chaîne de connexion, vous vous connectez à la base de données par défaut que l'administrateur vous a affectée. Contactez l'administrateur de votre base de données pour connaître les informations de connexion et d'identification à utiliser pour se connecter à la source de données. L'exemple de chaîne de connexion suivant spécifie l'exemple de base de données, **CustomerSales**, dans l'appliance [!INCLUDE[ssDW](../../includes/ssdw-md.md)] :  
  
```  
HOST=<IP address>; database= CustomerSales; port=<port>  
```  
  
 De plus, vous utilisez la boîte de dialogue **Propriétés de la source de données** pour fournir des informations d'identification telles que le nom d'utilisateur et le mot de passe, les options `User Id` et `Password` sont ajoutées automatiquement à la chaîne de connexion, vous n'avez pas besoin de les taper dans la chaîne de connexion. L'interface utilisateur fournit également des options permettant de spécifier l'adresse IP du nœud de contrôle dans l'appliance [!INCLUDE[ssDW](../../includes/ssdw-md.md)] , ainsi que le numéro de port. Par défaut, il s'agit du port 17000. Le port est configurable par un administrateur et votre chaîne de connexion peut utiliser un autre numéro de port.  
  
 Pour obtenir plus d’informations sur les exemples de chaînes de connexion, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Informations d'identification  
 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] fournit sa propre technologie de sécurité pour implémenter et stocker les noms d'utilisateurs et les mots de passe. Vous ne pouvez pas utiliser l'authentification Windows. Si vous essayez de vous connecter à [!INCLUDE[ssDW](../../includes/ssdw-md.md)] à l'aide de l'authentification Windows, une erreur se produit.  
  
 Les informations d'identification doivent être suffisantes pour accéder à la base de données. Selon votre requête, vous pouvez avoir besoin d'autres autorisations, par exemple des autorisations suffisantes pour accéder aux tables et aux vues. Le propriétaire de la source de données externe doit configurer des informations d'identification suffisantes pour fournir un accès en lecture seule aux objets de base de données nécessaires.  
  
 Sur un client de création de rapports, les options suivantes sont disponibles pour spécifier des informations d'identification :  
  
-   Utiliser un nom d'utilisateur et un mot de passe enregistrés. Pour négocier le double tronçon qui se produit lorsque la base de données qui contient les données de rapport est différente du serveur de rapports, sélectionnez les options permettant d'utiliser les informations d'identification en tant qu'informations d'identification Windows. Vous pouvez également choisir d'emprunter l'identité de l'utilisateur authentifié après la connexion à la source de données.  
  
-   Aucune information d'identification n'est requise. Pour utiliser cette option, vous devez avoir configuré le compte d'exécution sans assistance sur le serveur de rapports. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md). 
  
 Pour plus d’informations, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) ou [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="queries"></a><a name="Query"></a> Requêtes  
 Une requête spécifie les données à récupérer pour un dataset de rapport.  
  
 Les colonnes dans le jeu de résultats d'une requête remplissent la collection de champs pour un dataset. Si la requête retourne plusieurs jeux de résultats, le rapport traite uniquement le premier jeu de résultats qu'une requête récupère. Par défaut, si vous créez une requête ou si vous ouvrez une requête existante qui peut être représentée dans le concepteur de requêtes graphique, le concepteur de requêtes relationnelles est disponible. Vous pouvez spécifier une requête de plusieurs façons :  
  
-   Générer une requête de manière interactive. Utilisez le concepteur de requêtes relationnelles, qui affiche selon un mode hiérarchique les tables, vues et autres éléments de base de données, organisés par schéma de base de données. Sélectionnez des colonnes à partir des tables ou des vues. Limitez le nombre de lignes de données à récupérer en spécifiant des critères de filtre, des regroupements et des agrégats. Personnalisez le filtre lorsque le rapport s'exécute en définissant l'option de paramètre.  
  
-   Taper ou coller une requête. Utilisez le concepteur de requêtes textuel pour entrer directement du texte [!INCLUDE[DWsql](../../includes/dwsql-md.md)] , coller du texte de requête à partir d’une autre source, entrer des requêtes complexes impossibles à créer à l’aide du concepteur de requêtes relationnelles, ou entrer des expressions basées sur des requêtes.  
  
-   Importe une requête existante à partir d'un fichier ou d'un rapport. Utilisez le bouton **Importer une requête** à partir de l'un des concepteurs de requêtes afin de naviguer jusqu'à un fichier .sql ou .rdl, et d'importer une requête.  
  
 Pour plus d’informations, consultez [Interface utilisateur du Concepteur de requêtes relationnelles &#40;Générateur de rapports&#41;](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md) et [Interface utilisateur du Concepteur de requêtes textuel &#40;Générateur de rapports&#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 Le concepteur de requêtes textuel prend en charge le mode [Texte](#QueryText) dans lequel vous tapez des commandes [!INCLUDE[DWsql](../../includes/dwsql-md.md)] qui sélectionnent des données provenant de la source de données.  
  
-   [Text](#QueryText)  
  
 Vous utilisez [!INCLUDE[DWsql](../../includes/dwsql-md.md)] avec [!INCLUDE[ssDW](../../includes/ssdw-md.md)] et [!INCLUDE[tsql](../../includes/tsql-md.md)] avec SQL Server. Les deux dialectes du langage SQL sont très semblables. Les requêtes écrites pour le type de connexion à la source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être utilisées en général pour le type de connexion à la source de données [!INCLUDE[ssDWCurrentFull](../../includes/ssdwcurrentfull-md.md)] .  
  
 Une requête qui récupère des données de rapport à partir d'une base de données de grande taille, notamment un entrepôt de données tel que [!INCLUDE[ssDW](../../includes/ssdw-md.md)], peut générer un jeu de résultats qui possède un très grand nombre de lignes, sauf si vous agrégez et synthétisez les données afin de réduire le nombre des lignes retournées par la requête. Vous pouvez écrire des requêtes qui incluent des agrégats et des regroupements à l'aide du concepteur de requêtes graphique ou textuel.  
  
 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] prend en charge la clause, le mot clé et les agrégats que le concepteur de requêtes fournit pour synthétiser les données.  
  
 Le concepteur de requêtes graphique utilisé par [!INCLUDE[ssDW](../../includes/ssdw-md.md)] fournit une prise en charge intégrée du regroupement et des agrégats pour vous aider à écrire des requêtes qui récupèrent uniquement les données de synthèse. Les fonctionnalités du langage [!INCLUDE[DWsql](../../includes/dwsql-md.md)] sont les suivantes : clause GROUP BY, mot clé DISTINCT et agrégats tels que SUM et COUNT. Le concepteur de requêtes textuel fournit une prise en charge complète du langage [!INCLUDE[DWsql](../../includes/dwsql-md.md)] , notamment en matière de regroupement et d’agrégats.  
  
 Pour plus d’informations sur [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Référence Transact-SQL &#40;moteur de base de données&#41;](../../t-sql/language-reference.md).  
  
###  <a name="using-query-type-text"></a><a name="QueryText"></a> Utilisation du type de requête Texte  
 Dans le concepteur de requêtes textuel, vous tapez des commandes [!INCLUDE[DWsql](../../includes/dwsql-md.md)] pour définir les données d’un dataset. Les requêtes que vous utilisez pour récupérer des données de [!INCLUDE[ssDW](../../includes/ssdw-md.md)] sont les mêmes que celles que vous utilisez pour récupérer des données à partir d'instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne s'exécutent pas dans une application [!INCLUDE[ssDW](../../includes/ssdw-md.md)] . Par exemple, la requête [!INCLUDE[DWsql](../../includes/dwsql-md.md)] suivante sélectionne les noms de tous les employés qui occupent la fonction d'assistants marketing :  
  
```  
SELECT  
  HumanResources.Employee.BusinessEntityID  
  ,HumanResources.Employee.JobTitle  
  ,Person.Person.FirstName  
  ,Person.Person.LastName  
FROM  
  Person.Person  
  INNER JOIN HumanResources.Employee  
    ON Person.Person.BusinessEntityID = HumanResources.Employee.BusinessEntityID  
WHERE HumanResources.Employee.JobTitle = 'Marketing Assistant'   
```  
  
 Cliquez sur le bouton **Exécuter** ( **!** ) de la barre d’outils pour exécuter la requête et afficher un jeu de résultats.  
  
 Pour paramétrer cette requête, ajoutez un paramètre de requête. Par exemple, modifiez la clause WHERE comme suit :  
  
 `WHERE HumanResources.Employee.JobTitle = (@JobTitle)`  
  
 Lorsque vous exécutez la requête, les paramètres de rapport qui correspondent aux paramètres de requête sont créés automatiquement. Pour plus d'informations, consultez [Paramètres de requête](#Parameters) plus loin dans cette rubrique.  
  
  
##  <a name="parameters"></a><a name="Parameters"></a> Paramètres  
 Lorsque le texte de requête contient des variables de requête ou des procédures stockées dotées de paramètres d'entrée, les paramètres de requête correspondants pour le dataset et les paramètres de rapport pour le rapport sont automatiquement générés. Le texte de requête ne doit pas inclure l'instruction DECLARE pour chaque variable de requête.  
  
 Par exemple, la requête SQL suivante crée un paramètre de rapport nommé **EmpID**:  
  
```  
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```  
  
 Par défaut, chaque paramètre de rapport a le type de données Texte ; en outre, un dataset est créé automatiquement pour fournir une liste déroulante de valeurs disponibles. Après avoir créé les paramètres de rapport, vous devrez peut-être modifier les valeurs par défaut. Pour plus d'informations, consultez [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Notes  
  
###### <a name="platform-and-version-information"></a>Informations sur les plateformes et les versions  
 Pour plus d’informations sur la prise en charge des plateformes et des versions, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
  
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
  
 [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
 Fournit des informations détaillées sur la prise en charge des plateformes et des versions pour chaque extension de données.  

## <a name="next-steps"></a>Étapes suivantes

[Paramètres de rapport](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[Filtrer, regrouper et trier des données](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[Expressions](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)