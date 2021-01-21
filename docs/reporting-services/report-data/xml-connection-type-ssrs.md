---
title: Type de connexion XML | Microsoft Docs
description: Découvrez le type de connexion XML permettant de vous connecter et de récupérer des données à partir de documents XML, de services web ou de données XML incorporées dans la requête.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 5b55fff2-1b15-4156-83ef-15ad9cf9f509
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b6ca1df39a5aeed8b57de5c6eff7ab66e725062d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596532"
---
# <a name="xml-connection-type-ssrs"></a>Type de connexion XML (SSRS)
  Pour inclure les données d'une source de données XML dans votre rapport, vous devez avoir un dataset basé sur une source de données de rapport de type XML. Ce type de source de données intégré est basé sur l'extension de données XML. Ce type de source de données vous permet de vous connecter et de récupérer des données à partir de documents XML, de services Web ou de données XML incorporées dans la requête.  
  
 Cette extension de données prend en charge les paramètres et les informations d'identification gérés indépendamment de la chaîne de connexion.  
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Chaîne de connexion  
 La chaîne de connexion doit être une URL qui pointe vers le service Web, l'application Web ou le document XML disponible via HTTP. Les documents XML doivent disposer de l'extension XML. Vous pouvez également utiliser une chaîne de connexion vide pour les données XML incorporées dans la requête du dataset.  
  
 Les exemples suivants illustrent la syntaxe d'une chaîne de connexion pour un service Web ou pour un document XML. Le protocole `file://` n'est pas pris en charge.  
  
|Type de document XML|Exemple de chaîne de connexion|  
|-----------------------|-------------------------------|  
|Service Web|`https://adventure-works.com/results.aspx`|  
|Document XML|`https://localhost/XML/Customers.xml`|  
|Document XML incorporé|*Vide*|  
  
 Pour obtenir d’autres exemples sur les chaînes de connexion, consultez [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Informations d'identification  
 Les informations d'identification sont obligatoires pour exécuter des requêtes, afficher l'aperçu du rapport localement et afficher l'aperçu du rapport à partir du serveur de rapports.  
  
 Après avoir publié votre rapport, vous pouvez devoir modifier les informations d'identification pour la source de données afin que les autorisations soient valides pour récupérer les données lorsque le rapport s'exécute sur le serveur de rapports.  
  
 Sur un client de création de rapports, les options suivantes sont disponibles pour spécifier des informations d'identification :  
  
-   Utilisateur Windows actuel (également appelé sécurité intégrée).  
  
-   Aucune information d'identification n'est requise. Si vous choisissez de ne pas demander d'informations d'identification, l'accès anonyme est utilisé. Vérifiez que vous avez défini le compte d'exécution sans assistance pour le serveur de rapports afin d'établir une connexion à une source de données externe. L'extension pour le traitement des données XML ne transmet pas d'informations d'identification à l'URL cible ou au service Web ; la connexion ne peut aboutir que si vous avez défini le compte d'exécution sans assistance. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
 Les informations d'identification stockées ou demandées ne sont pas prises en charge. Notez bien que si vous désactivez la sécurité intégrée de Windows, vous ne pouvez pas l'utiliser pour récupérer des données. Si vous spécifiez des informations d'identification stockées ou demandées par invite, une erreur se produit au moment de l'exécution.  
  
 Pour plus d’informations, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) ou [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](specify-credential-and-connection-information-for-report-data-sources.md).  
  
##  <a name="queries"></a><a name="Query"></a> Requêtes  
 Une requête spécifie les données à récupérer pour un dataset de rapport. Les colonnes dans le jeu de résultats d'une requête remplissent la collection de champs pour un dataset. Un rapport traite uniquement le premier jeu de résultats récupéré par une requête.  
  
 Vous devez utiliser le concepteur de requêtes textuel pour créer la requête. La requête doit retourner des données XML.  
  
 Pour plus d’informations sur le concepteur de requêtes textuel, consultez [Interface utilisateur du Concepteur de requêtes textuel &#40;Générateur de rapports&#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 Les valeurs possibles d’une requête de jeu de données pour une source de données de type XML sont indiquées ci-dessous.  
  
-   *Vide*  
  
     Utilisez une requête vide pour créer un ensemble de résultats par défaut. La requête par défaut est créée en lisant la source de données et en parcourant la hiérarchie de nœuds XML jusqu'à la première collection inférieure. L'ensemble de résultats inclut tous les nœuds avec des valeurs de texte et tous les attributs de nœud rencontrés sur ce parcours. Les colonnes dans l'ensemble de résultats sont mappées avec les champs du dataset.  
  
-   *Chemin d'accès à l'élément*  
  
     Spécifie la séquence des nœuds à utiliser lors de l'extraction des données XML de la source de données.  
  
-   *Élément de requête XML*  
  
     Spécification de requête XML avec les éléments facultatifs suivants :  
  
    -   **La source de données XML est un service web**  
  
         Éléments XML obligatoires :  
  
         `<Method Namespace=` *« espace de noms »*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *action soap* `</SoapAction>`  
  
         Éléments XML facultatifs :  
  
         `<ElementPath>`  *chemin de l’élément*  `</ElementPath>`  
  
         `<Method Namespace=` *« espace de noms »*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *action soap* `</SoapAction>`  
  
    -   **La source de données XML est un document XML**  
  
         Éléments XML obligatoires : None  
  
         Éléments XML facultatifs :  
  
         `<ElementPath>`  *chemin de l’élément*  `</ElementPath>`  
  
    -   **La source de données XML est un document XML incorporé**  
  
         Éléments XML obligatoires :  
  
         `<XmlData>` XML interne `</XmlData>`  
  
         Éléments XML facultatifs :  
  
         `<ElementPath>`  *chemin de l’élément*  `</ElementPath>`  
  
         `-- or --`  
  
         `<ElementPath IgnoreNamespaces="true">`  *chemin de l’élément*  `</ElementPath>`  
  
 Pour plus d’informations sur la syntaxe de requête, consultez [Syntaxe de requête XML pour les données de rapport XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-query-syntax-for-xml-report-data-ssrs.md).  
  
 Pour obtenir des exemples, consultez [Reporting Services : Utilisation de sources de données de services web et XML](/previous-versions/sql/sql-server-2005/administrator/aa964129(v=sql.90)).  
  
### <a name="requirements-for-retrieving-xml-web-service-data"></a>Configuration requise pour la récupération de données d'un service Web XML  
 L'extension pour le traitement des données XML ne détecte pas le schéma automatiquement. Vous devez donc disposer d'un moyen d'identifier quelles méthodes SOAP permettent d'extraire les données de votre choix. Vous devez également comprendre le schéma d'adressage ou l'espace de noms que le service Web utilise pour ses données.  
  
 Pour un service web, vous pouvez fournir un élément \<**Query**> qui spécifie une méthode à appeler ou une action SOAP. Vous pouvez laisser la requête vide et adopter la requête par défaut si la source de données XML dispose d'une structure hiérarchique produisant les données que vous souhaitez utiliser dans votre rapport. Les valeurs de nœud d'élément XML et les attributs récupérés lors de l'exécution de la requête correspondent aux champs de dataset que vous utilisez dans votre rapport.  
  
### <a name="requirements-for-retrieving-xml-document-data"></a>Conditions requises pour la récupération de données de documents XML  
 À l'aide du protocole HTTP, le serveur doit retourner les données XML ou ces dernières doivent être incorporées dans l'élément **Query** XML. Si vous faites directement référence à un document XML à l'aide du protocole HTTP, l'extension employée doit être l'extension .xml.  
  
 Vous devez savoir comment créer une requête XML qui récupère toutes les données dont vous avez besoin. Si vous ne spécifiez pas un chemin d'accès à l'élément, le comportement par défaut pour l'analyse d'un document XML consiste à sélectionner le premier chemin d'accès disponible vers une collection de nœuds terminaux dans le document. Si le document XML inclut des chemins d'accès supplémentaires à d'autres collections sœurs de nœuds terminaux (inférieurs), ces nœuds seront ignorés sauf si vous spécifiez un chemin dans votre requête.  
  
 Vous pouvez préciser un chemin d'accès de l'élément à l'aide d'une syntaxe XML semblable à la syntaxe XQuery.  
  
 Pour plus d’informations, consultez [Syntaxe du chemin d’accès à l’élément pour des données de rapport XML &#40;SSRS&#41;](../../reporting-services/report-data/element-path-syntax-for-xml-report-data-ssrs.md).  
  
##  <a name="parameters"></a><a name="Parameters"></a> Paramètres  
 La requête n'est pas analysée pour l'identification des paramètres.  
  
 Pour ajouter des paramètres, vous devez les créer manuellement via la page **Paramètre** dans la boîte de dialogue [Propriétés du dataset](/previous-versions/sql/) .  
  
##  <a name="remarks"></a><a name="Remarks"></a> Notes  
 L'extension de données XML prend en charge la création de rapports à partir de données XML tabulaires et non hiérarchiques. Pour plus d’informations, consultez [Ajouter des données à partir de sources de données externes &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md).  
  
 Il n'existe pas de prise en charge intégrée pour la récupération de documents XML à partir d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
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
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
