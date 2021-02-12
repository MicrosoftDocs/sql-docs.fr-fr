---
title: Type de connexion de liste SharePoint | Microsoft Docs
description: Utilisez les informations de cet article sur le type de connexion de liste SharePoint pour savoir comment créer une source de données.
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 2c4adf2f-e9c4-4fae-bd3c-97fe64436caf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7bb6380e168c1e4bddefab539354d80bf14805c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033459"
---
# <a name="sharepoint-list-connection-type-ssrs"></a>Type de connexion de liste SharePoint (SSRS)

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)] [!INCLUDE [ssrs-appliesto-sharepoint-2013-2016](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

Pour inclure des données d'une liste Microsoft SharePoint dans votre rapport, vous devez ajouter ou créer un dataset basé sur une source de données de rapport de type Liste Microsoft SharePoint. Il s'agit d'un type de source de données intégré basé sur l'extension de données Liste SharePoint Microsoft SQL Server Reporting Services. Utilisez ce type de source de données pour vous connecter à des données de liste provenant de SharePoint 2013 et ultérieur et les récupérer.

Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  

##  <a name="connection-string"></a><a name="Connection"></a> Chaîne de connexion  
 La chaîne de connexion à une liste SharePoint est l'URL pointant vers le site ou sous-site SharePoint, par exemple `https://MySharePointWeb/MySharePointSite` ou `https://MySharePointWeb/MySharePointSite/Subsite`.  
  
 Le concepteur de requêtes affiche automatiquement les listes SharePoint pour lesquelles vous disposez d'autorisations d'accès suffisantes.  
  
 Pour obtenir d’autres exemples sur les chaînes de connexion, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Informations d'identification  
 Les informations d'identification sont obligatoires pour exécuter des requêtes, afficher l'aperçu du rapport localement et afficher l'aperçu du rapport à partir du serveur de rapports. Après avoir publié votre rapport, vous pouvez devoir modifier les informations d'identification pour la source de données afin que les autorisations soient valides pour récupérer les données lorsque le rapport s'exécute sur le serveur de rapports. Les types d'informations d'identification qui peuvent être utilisés avec cette extension de données dépendent de la configuration de la technologie SharePoint pour la liste SharePoint que vous utilisez comme source de données.  
  
 Les tableaux suivants décrivent le comportement de récupération des informations d'identification pour l'extension de liste SharePoint lors de la connexion à une liste SharePoint locale de batteries de serveurs et à une liste SharePoint distante.  
  
 Le **tableau 1** dresse la liste des rapports déployés sur un site Windows SharePoint hérité. Un site Windows hérité prend en charge uniquement l'authentification Kerberos, NTLM et à base de formulaires (FBA). Le **tableau 2** dresse la liste des rapports déployés sur un site SharePoint basé sur les revendications.  
  
 **Tableau 1**  
  
|Type de récupération|Informations d'identification prises en charge|Mode d'authentification Windows classique|*Authentification par revendications|  
|-|---------------------------|-----------------------------------------|-----------------------------|  
|Liste SharePoint locale de batteries de serveurs|Jeton utilisateur pour l'authentification Windows (intégrée) ou SharePoint|Oui|Oui|  
||Stockage, invite, aucun (avec les informations d’identification Windows)<br /><br /> Les informations d’identification stockées et sur invite non-Windows ne sont pas prises en charge.|Oui|Non|  
|Liste SharePoint distante|Jeton utilisateur pour l'authentification Windows (intégrée) ou SharePoint|Oui|Non<br /><br /> L’authentification basée sur des formulaires et l’authentification par revendications ne sont pas prises en charge pour les listes SharePoint distantes.|  
||Stockage, invite, aucun (avec les informations d’identification Windows)<br /><br /> Les informations d’identification stockées et sur invite non-Windows ne sont pas prises en charge.|Oui|Non<br /><br /> L’authentification basée sur des formulaires et l’authentification par revendications ne sont pas prises en charge pour les listes SharePoint distantes.|  
  
 *Authentification Windows, authentification basée sur des formulaires (FBA), jetons SAML (Secure Application Markup Language), autres fournisseurs d’identité ou combinaison de plusieurs fournisseurs d’authentification indiqués ci-dessus.  
  
 **Tableau 2**  
  
|Type de récupération|Informations d'identification prises en charge|Mode d'authentification Windows classique|*Authentification par revendications|  
|-|---------------------------|-----------------------------------------|-----------------------------|  
|Liste SharePoint locale de batteries de serveurs|Jeton utilisateur pour l'authentification Windows (intégrée) ou SharePoint|Oui|Oui|  
||Stockage, invite, aucun (avec les informations d’identification Windows)<br /><br /> Les informations d’identification stockées et sur invite non-Windows ne sont pas prises en charge.|Non|Non|  
|Liste SharePoint distante|Jeton utilisateur pour l'authentification Windows (intégrée) ou SharePoint|Oui|Non<br /><br /> L’authentification basée sur des formulaires et l’authentification par revendications ne sont pas prises en charge pour les listes SharePoint distantes.|  
||Stockage, invite, aucun (avec les informations d’identification Windows)<br /><br /> Les informations d’identification stockées et sur invite non-Windows ne sont pas prises en charge.|Non|Non<br /><br /> L’authentification basée sur des formulaires et l’authentification par revendications ne sont pas prises en charge pour les listes SharePoint distantes.|  
  
 *Authentification Windows, authentification basée sur des formulaires (FBA), jetons SAML (Secure Application Markup Language), autres fournisseurs d’identité ou combinaison de plusieurs fournisseurs d’authentification indiqués ci-dessus.  
  
 **Authentification Windows**  
 Pour une technologie SharePoint configurée pour être utilisée avec un serveur de rapports en mode Compte approuvé, cette option n’est pas prise en charge. Cela s'applique uniquement aux versions antérieures à SQL Server 2012 Reporting Services.

 Pour une technologie SharePoint configurée pour être utilisée avec un serveur de rapports en mode intégré Windows, cette option s'applique à la fois à l'utilisateur Windows actuel et à l'utilisateur SharePoint actuel.
 
 Pour une technologie SharePoint configurée pour être utilisée sans serveur de rapports (mode local), cette option n'est pas prise en charge. Pour plus d’informations sur le mode local, consultez [Rapports en mode local et rapports en mode connecté dans la Visionneuse de rapports &#40;Reporting Services en mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md).  
  
 **Informations d'identification non requises (ne pas utiliser d'informations d'identification) :**  
 Pour utiliser cette option, vous devez avoir configuré le compte d'exécution sans assistance sur le serveur de rapports. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
 Pour plus d'informations sur la prise en charge de l'authentification par revendications dans la pile Microsoft BI, consultez [Utilisation de l'authentification par revendications dans la pile Microsoft BI](https://social.technet.microsoft.com/wiki/contents/articles/15274.using-claims-authentication-across-the-microsoft-bi-stack.aspx).  
  
 Pour plus d’informations, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md), [Spécifier des informations d'identification et de connexion pour les sources de données de rapport](specify-credential-and-connection-information-for-report-data-sources.md) et [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
##  <a name="queries"></a><a name="Query"></a> Requêtes  
 Pour concevoir une requête, créez un dataset selon la source de données, puis ouvrez le concepteur de requêtes associé. Pour plus d’informations, consultez [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md).  
  
 Le concepteur de requêtes graphique de liste SharePoint affiche quatre volets :  
  
 **Listes SharePoint**  Affiche la liste de toutes les listes SharePoint sur le site pour cette source de données. Sélectionnez une liste, puis sélectionnez les champs que vous souhaitez dans votre requête. Les noms des champs dans ce volet sont les noms conviviaux SharePoint, également appelés noms complets. Pointez sur un élément pour afficher les propriétés suivantes dans l'info-bulle :  
  
-   **Nom** Nom unique du champ.  
  
-   **Identificateur** Identificateur unique du champ.  
  
-   **Type de champ** Type de données du champ.  
  
-   **Caché** Indique si le champ s'affiche dans l'affichage des listes SharePoint.  
  
 La sélection de champs dans plusieurs listes n'est pas prise en charge. Vous pouvez créer un dataset pour chaque liste et sélectionner des champs dans chaque dataset. Si les listes ont un champ en commun, vous pouvez utiliser la fonction de recherche dans une région de données de tableau matriciel liée à l'un des deux dataset pour extraire une valeur de l'autre dataset qui n'est pas lié à la région de données. Pour plus d’informations, consultez [Fonction de recherche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookup-function.md).  
  
-   **Champs sélectionnés**  Affiche les champs que vous avez sélectionnés. Les noms des champs dans ce volet sont des noms conviviaux qu'un utilisateur SharePoint a spécifiés. Lorsque vous fermez le concepteur de requêtes, vous voyez ces noms dans la collection de champs de dataset dans le volet des données de rapport. La relation entre les noms uniques et les noms conviviaux est disponible dans la page [Boîte de dialogue Propriétés du dataset, Champs &#40;Générateur de rapports&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md).  
  
-   **Filtres appliqués**  Limite les données retournées à partir de la liste SharePoint, avant que les données ne soient retournées vers le rapport. Sélectionnez le nom de champ, l'opérateur et la valeur à utiliser pour limiter les données récupérées dans la liste. Les opérateurs varient selon le type de données de la valeur que vous sélectionnez.  
  
     Vous ne pouvez pas modifier l'ordre de tri ni spécifier des groupes dans le concepteur de requêtes graphique. Pour ce faire, définissez des expressions de tri sur le dataset du rapport et des expressions de groupe sur les régions de données dans le rapport. Les paramètres de requête ne sont pas pris en charge. Pour filtrer des données dans le rapport, utilisez des filtres de rapport ou paramètres de rapport que vous créez. Pour plus d’informations, consultez [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md) et [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
-   **Résultats de la requête**  Affiche des lignes exemples qui sont retournées lors de l'exécution de la requête. Si les valeurs de liste SharePoint changent fréquemment sur le site SharePoint, les valeurs que vous voyez dans le volet des résultats de la requête peuvent différer de celles que vous voyez dans le rapport.  
  
-   **Champs sélectionnés**  Affiche les champs que vous avez sélectionnés. Les noms des champs dans ce volet sont des noms conviviaux qu'un utilisateur SharePoint a spécifiés. Lorsque vous fermez le concepteur de requêtes, vous voyez ces noms dans la collection de champs de dataset dans le volet des données de rapport. La relation entre les noms uniques et les noms conviviaux est disponible dans la page [Boîte de dialogue Propriétés du dataset, Champs &#40;Générateur de rapports&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md).  
  
-   **Filtres appliqués**  Limite les données retournées à partir de la liste SharePoint, avant que les données ne soient retournées vers le rapport. Sélectionnez le nom de champ, l'opérateur et la valeur à utiliser pour limiter les données récupérées dans la liste. Les opérateurs varient selon le type de données de la valeur que vous sélectionnez.  
  
     Vous ne pouvez pas modifier l'ordre de tri ni spécifier des groupes dans le concepteur de requêtes graphique. Pour ce faire, définissez des expressions de tri sur le dataset du rapport et des expressions de groupe sur les régions de données dans le rapport. Les paramètres de requête ne sont pas pris en charge. Pour filtrer des données dans le rapport, utilisez des filtres de rapport ou paramètres de rapport que vous créez. Pour plus d’informations, consultez [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md) et [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
-   **Résultats de la requête**  Affiche des lignes exemples qui sont retournées lors de l'exécution de la requête. Si les valeurs de liste SharePoint changent fréquemment sur le site SharePoint, les valeurs que vous voyez dans le volet des résultats de la requête peuvent différer de celles que vous voyez dans le rapport.  
  
 Pour plus d’informations, consultez [Concepteur de requêtes de liste SharePoint &#40;Générateur de rapports&#41;](../../reporting-services/report-data/sharepoint-list-query-designer-report-builder.md).  
  
### <a name="query-text"></a>Texte de la requête  
 Pour afficher la requête générée par le concepteur de requêtes graphique, basculez vers le concepteur de requêtes textuel. Dans ce mode, vous pouvez voir le code XML qui est créé par le concepteur de requêtes graphique. Le code XML inclut des éléments pour le nom de liste, la collection de champs et le filtre.  
  
#### <a name="example-1-specified-fields-for-a-list"></a>Exemple 1. Champs spécifiés pour une liste  
 L'exemple suivant illustre une requête SharePoint bien formée :  
  
```  
<RSSharePointList>  
<listName>MyList</listName>  
<viewFields>  
  <FieldRef Name="Field1"/>  
  <FieldRef Name="Field4"/>  
</viewFields>  
<Query>  
  <Where>  
    <And>  
      <Gt>  
        <FieldRef Name="Field1"/>  
        <Value Type="Integer">1</Value>  
      </Gt>  
      <IsNotNull>  
        <FieldRef Name="Field2"/>  
        <Value Type="string"/>  
      </IsNotNull>   
    </And>  
  </Where>  
</Query>  
</RSSharePointList>  
```  
  
 Vous pouvez modifier cet affichage de la requête tant qu'elle se présente sous la forme de texte XML bien formé.  
  
#### <a name="example-2-all-fields-for-a-list"></a>Exemple 2. Tous les champs pour une liste  
 Vous pouvez également spécifier uniquement le nom d'une liste, et tous les champs, notamment les champs masqués, sont retournés. L'exemple suivant récupère tous les champs dans une liste nommée Tasks :  
  
```  
<RSSharePointList>  
<listName>Tasks</listName>  
</RSSharePointList>  
```  
  
 Tous les champs de la liste Tasks sont retournés dans les résultats de la requête.  
  
##  <a name="parameters"></a><a name="Parameters"></a> Paramètres  
 Les paramètres ne sont pas pris en charge par cette extension de données.  
  
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

## <a name="see-also"></a>Voir aussi

[Paramètres de rapport](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[Filtrer, regrouper et trier des données](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
