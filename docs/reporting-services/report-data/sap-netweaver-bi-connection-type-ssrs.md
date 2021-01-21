---
title: Type de connexion SAP NetWeaver BI | Microsoft Docs
description: Utilisez les informations de cet article sur le type de connexion SAP NetWeaver BI pour savoir comment créer une source de données.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: f985856b-31d5-4e56-844b-8a8ee38da67e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4b376fddd69a44f9280d4c0aaec4f58880d9ff76
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596487"
---
# <a name="sap-netweaver-bi-connection-type-ssrs"></a>Type de connexion SAP NetWeaver BI (SSRS)
  Pour inclure les données d'une source de données externe SAP NetWeaver® Business Intelligence dans votre rapport, vous devez avoir un dataset basé sur une source de données de rapport de type [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)]. Ce type de source de données intégré est basé sur l'extension de données du fournisseur de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework 1.0 pour [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)].  
  
 Cette extension de données vous permet de récupérer des données multidimensionnelles à partir de requêtes InfoCubes, MultiProviders (InfoCubes virtuels) et web définies sur une source de données externe [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] .  
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Chaîne de connexion  
 Contactez l'administrateur de la base de données pour connaître les informations de connexion et d'identification à utiliser pour se connecter à la source de données. L’exemple de chaîne de connexion suivant spécifie une source de données [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] sur un serveur utilisant le port 8000 et XMLA (XML for Analysis Services) sur Internet avec SOAP :  
  
```  
DataSource=https://mySAPNetWeaverBIServer:8000/sap/bw/xml/soap/xmla  
```  
  
 Pour obtenir d’autres exemples sur les chaînes de connexion, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
  
##  <a name="credentials"></a><a name="Credentials"></a> Informations d'identification  
 Les informations d'identification sont obligatoires pour exécuter des requêtes, afficher l'aperçu du rapport localement et afficher l'aperçu du rapport à partir du serveur de rapports.  
  
 Après avoir publié votre rapport, vous pouvez devoir modifier les informations d'identification pour la source de données afin que les autorisations soient valides pour récupérer les données lorsque le rapport s'exécute sur le serveur de rapports.  
  
 Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="queries"></a><a name="Query"></a> Requêtes  
 Vous pouvez utiliser le concepteur de requêtes graphique en mode Création ou en mode Requête pour générer une requête MDX (Multidimensional Expression) en parcourant les structures de données sous-jacentes de la source de données. Lors de la conception, vous pouvez exécuter interactivement une requête à partir du Concepteur de requêtes pour voir les résultats. La requête que vous créez définit les champs du dataset. Lors de l'exécution, les données réelles sont retournées à partir de la source de données. Utilisez le concepteur de requêtes graphique pour effectuer les actions suivantes :  
  
-   En mode Création, faites glisser les dimensions, les membres, les propriétés de membre et les chiffres clés de la source de données vers le volet Données pour créer une requête MDX (Multidimensional Expression). Faites glisser les membres calculés du volet Membres calculés vers le volet Données pour définir d'autres champs du dataset.  
  
-   En mode Requête, faites glisser les dimensions, les membres, les propriétés de membre et les chiffres clés vers le volet Requête, ou tapez le texte MDX directement dans le volet Requête. Faites glisser les membres calculés du volet Membres calculés vers le volet Données pour définir d'autres champs du dataset.  
  
 Pendant que vous générez des requêtes, le concepteur de requêtes ajoute automatiquement des propriétés par défaut à la requête MDX. Pour inclure des propriétés autres que les propriétés par défaut, vous devez modifier manuellement la requête MDX.  
  
 Pour plus d’informations sur l’utilisation de ce Concepteur de requêtes, consultez [Interface utilisateur du Concepteur de requêtes SAP NetWeaver BI &#40;Générateur de rapports&#41;](/previous-versions/sql/).  
  
  
##  <a name="extended-field-properties"></a><a name="Extended"></a> Propriétés de champ étendues  
 La source de données [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] prend en charge les propriétés de champ étendues. Les propriétés de champ étendues sont des propriétés complémentaires à **Value** et **IsMissing** qui sont définies pour un champ de dataset par l'extension pour le traitement des données. Les propriétés étendues incluent des propriétés prédéfinies et des propriétés personnalisées. Les propriétés prédéfinies sont des propriétés communes à plusieurs sources de données. Les propriétés personnalisées sont uniques à chaque source de données.  
  
### <a name="working-with-field-properties"></a>Utilisation des propriétés de champ  
 Les propriétés de champ étendues n'apparaissent pas dans le volet des données de rapport en tant qu'éléments que vous pouvez faire glisser vers votre disposition de rapport. À la place, vous faites glisser le champ parent de la propriété sur le rapport, puis vous remplacez la propriété par défaut **Value** par la propriété que vous voulez utiliser. Par exemple, si le nom de champ **Calendar Year/Month Level 01** est créé dans un Concepteur de requêtes MDX en déplaçant un niveau du volet de métadonnées vers le volet de requête, vous faites référence à la propriété d’étendue personnalisée **Long name** (Nom long) dans une expression à l’aide de la syntaxe suivante :  
  
 `=Fields!Calendar_Year_Month_Level_01("Long Name")`  
  
 Le nom d'une propriété de champ étendue apparaît dans l'info-bulle lorsque vous placez le pointeur sur un champ dans le volet de métadonnées. Pour plus d'informations sur les concepteurs de requêtes que vous pouvez utiliser pour explorer les données sous-jacentes, consultez [SAP NetWeaver BI Query Designer User Interface](../../reporting-services/report-data/sap-netweaver-bi-query-designer-user-interface.md).  
  
> [!NOTE]  
>  Il existe des valeurs pour les propriétés de champ étendues uniquement si la source de données fournit ces valeurs lorsque votre rapport s'exécute et récupère les données pour ses datasets. Vous pouvez alors faire référence à ces valeurs de propriété **Field** à partir d'une expression en utilisant la syntaxe décrite ci-dessous. Cependant, dans la mesure où ces champs sont spécifiques à ce fournisseur de données et ne font pas partie du langage de définition de rapport, les modifications que vous apportez à ces valeurs ne sont pas enregistrées avec la définition du rapport.  
  
 Pour faire référence à des propriétés étendues prédéfinies dans une expression, vous pouvez utiliser l'une des syntaxes décrites ci-dessous :  
  
-   *Fields!FieldName.PropertyName*  
  
-   *Fields!FieldName("PropertyName")*  
  
 Pour faire référence à des propriétés étendues personnalisées dans une expression, vous pouvez utiliser la syntaxe suivante :  
  
-   *Fields!FieldName("PropertyName")*  
  
  
### <a name="predefined-field-properties"></a>Propriétés de champ prédéfinies  
 Le tableau suivant dresse la liste des propriétés de champ prédéfinies pouvant être utilisées pour une source de données [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] .  
  
|**Propriété**|**Type**|**Description ou valeur attendue**|  
|------------------|--------------|---------------------------------------|  
|**Valeur**|**Object**|Précise la valeur de données du champ.|  
|**IsMissing**|**Booléen**|Indique si le champ figure dans le dataset obtenu.|  
|**FormattedValue**|**Chaîne**|Retourne la valeur mise en forme d'un élément clé.|  
|**BackgroundColor**|**Chaîne**|Retourne la couleur d'arrière-plan définie dans la base de données pour le champ.|  
|**Color**|**Chaîne**|Retourne la couleur de premier plan définie dans la base de données pour l'élément.|  
|**Clé**|**Object**|Retourne la clé d'un niveau.|  
|**LevelNumber**|**Integer**|Dans le cas des hiérarchies parent-enfant, cette propriété retourne le nombre de niveaux ou de dimensions.|  
|**ParentUniqueName**|**Chaîne**|Dans le cas des hiérarchies parent-enfant, cette propriété retourne le nom complet du niveau parent.|  
|**UniqueName**|**Chaîne**|Retourne le nom complet d'un niveau. Par exemple, la valeur de **UniqueName** pour un employé peut être *[0D_Company].[10D_Department]. [11]* .|  
  
 Pour plus d’informations sur l’utilisation de champs et de propriétés de champ dans une expression, consultez [Collections intégrées dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md).  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Notes  
 Certains modes de remise de rapport ne sont pas pris en charge par ce fournisseur de données. La remise des rapports par le biais d'abonnements pilotés par les données n'est pas prise en charge pour cette extension pour le traitement des données. Pour plus d’informations, consultez [Utiliser une source de données externe pour les données des abonnés &#40;abonnements pilotés par les données&#41;](../../reporting-services/subscriptions/use-an-external-data-source-for-subscriber-data-data-driven-subscription.md).  
  
 Pour plus d'informations, consultez [Utilisation de SQL Server 2008 Reporting Services avec SAP NetWeaver Business Intelligence](/previous-versions/sql/sql-server-2008/cc974473(v=sql.100)).  
  
  
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
  
 [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
 Fournit des informations détaillées sur la prise en charge des plateformes et des versions pour chaque extension de données.  
  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
