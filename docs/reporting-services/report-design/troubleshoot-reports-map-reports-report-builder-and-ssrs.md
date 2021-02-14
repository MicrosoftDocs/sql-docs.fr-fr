---
title: 'Dépanner les rapports : Rapports cartographiques (Générateur de rapports) | Microsoft Docs'
description: Explorez les idées de résolution des problèmes qui peuvent se produire quand vous personnalisez, ajoutez une couche ou publiez un rapport avec une carte dans le Générateur de rapports.
ms.date: 07/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a690aec2-056b-40bc-8cab-c694bd2d6d62
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a4580559356869ffb52d75241219e42c8511ba3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081160"
---
# <a name="troubleshoot-reports-map-reports-report-builder-and-ssrs"></a>Dépanner les rapports : Rapports cartographiques (Générateur de rapports et SSRS)
  Vous pouvez rencontrer des problèmes quand vous ajoutez une carte ou une couche à votre rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , quand vous personnalisez une carte ou une couche existante de votre rapport, quand vous affichez l’aperçu d’une carte dans un rapport ou quand vous publiez un rapport contenant une carte. Utilisez cette rubrique pour vous aider à résoudre ces problèmes.  
    
   ## <a name="need-more-help"></a>Besoin d’aide ?  
   
  Essayez de procéder comme suit :  
 * [SQL Server Reporting Services](https://stackoverflow.com/questions/tagged/reporting-services) sur Stack Overflow  
 * Signalez un problème ou faites une suggestion sur [Microsoft SQL Server UserVoice](https://feedback.azure.com/forums/908035-sql-server).  

  
##  <a name="report-definition-size-issues"></a><a name="Embedded"></a> Problèmes liés à la taille de la définition de rapport  
 Utilisez cette section pour résoudre des problèmes liés à la taille de la définition de rapport.  
  
## <a name="how-do-i-reduce-the-report-definition-size"></a>Comment puis-je réduire la taille de la définition de rapport ?  
 Une couche contient des éléments cartographiques créés à partir de données spatiales. Dans certains cas, les éléments cartographiques sont incorporés dans la définition de rapport. Ceci est le cas dans les scénarios suivants :  
  
-   Si la source de données spatiales provient d'une carte de la bibliothèque de cartes ou d'un fichier de forme ESRI situé sur votre ordinateur local, les éléments cartographiques sont incorporés automatiquement dans la définition de rapport.  
  
     Si vous publiez un rapport sur le serveur de rapports et qu'il existe une référence de source de données spatiales à un fichier local, les données spatiales ne peuvent pas être récupérées lors du traitement du rapport. Pour éviter ce problème, les données de la carte sont incorporées dans la définition de rapport.  
  
-   Dans l'Assistant Carte ou l'Assistant Couche, si vous sélectionnez l'option permettant d'incorporer les données spatiales, les éléments cartographiques basés sur les données spatiales sont incorporés dans la couche dans la définition de rapport.  
  
-   Dans le volet Carte, si vous cliquez avec le bouton droit sur la couche, puis sélectionnez l’une des options **Incorporer les données spatiales** , les éléments cartographiques basés sur les données spatiales sont incorporés dans la couche dans la définition de rapport.  
  
-   Pour supprimer d'une définition de rapport des données incorporées basées sur un fichier de forme ESRI, vous devez effectuer les opérations suivantes :  
  
1.  Téléchargez ou publiez les fichiers ESRI .shp et .dbf sur le serveur de rapports.  
  
2.  Dans le rapport, dans le volet Carte en mode Conception, sélectionnez la couche contenant des données incorporées et ouvrez la boîte de dialogue de propriétés **Données de couche** . Dans **Utiliser les données spatiales de**, sélectionnez **Lien vers le fichier de forme ESRI**, recherchez sur le serveur de rapports le dossier qui contient les fichiers de forme ESRI, sélectionnez-le et cliquez sur OK.  
  
3.  Enregistrez votre rapport. Les données incorporées de la couche que vous avez modifiée ont été supprimées de la définition de rapport.  
  
 Les éléments cartographiques provenant d'un rapport de la bibliothèque de cartes seront toujours incorporés dans une couche.  
  
##  <a name="spatial-data-issues"></a><a name="Spatial"></a> Problèmes liés aux données spatiales  
 Utilisez cette section pour résoudre des problèmes liés aux données spatiales.  
  
## <a name="on-the-design-surface-i-see-sample-spatial-data"></a>Des exemples de données spatiales sont affichés sur l'aire de conception.  
 Lors de la conception, l'aire de conception peut afficher un message concernant des exemples de données spatiales pour les raisons suivantes :  
  
-   Les données spatiales proviennent d'un fichier ESRI .shp, mais le fichier .dbf correspondant n'est pas disponible. Les fichiers de forme ESRI incluent normalement à la fois un fichier .shp avec des données spatiales et un fichier de support .dbf. Vérifiez que le fichier .dbf est dans le même dossier que le fichier .shp.  
  
-   Les données spatiales proviennent d'un dataset et la connexion de données pour la requête n'est pas valide ou les informations d'identification actuelles ne sont pas valides.  
  
-   La couche contient une propriété avec une expression. Les expressions ne sont pas évaluées jusqu'à ce que le rapport s'exécute. Pour afficher la carte, vous devez afficher un aperçu du rapport.  
  
-   Les données spatiales proviennent d'un dataset qui a une étendue spécifique. Par exemple, lorsqu'une carte est imbriquée dans une région de données de tableau matriciel, ou que la carte utilise la même étendue de dataset pour les données analytiques et spatiales, l'étendue de données n'est pas calculée jusqu'à ce que le rapport s'exécute.  
  
## <a name="when-i-set-an-offset-for-an-individual-map-element-a-cluster-of-map-elements-move"></a>Lorsque je définis un décalage pour un élément cartographique individuel, un cluster d'éléments cartographiques est déplacé.  
 Les données spatiales définissent les éléments cartographiques affichés sur chaque couche. Un élément cartographique peut être basé sur des données spatiales correspondant à un point unique, un ensemble de points, une ligne unique, un ensemble de lignes, un polygone unique ou un ensemble de polygones. Chaque élément cartographique représente une unité. Si un élément cartographique contient plusieurs points et que vous déplacez l'élément, tous les points de cet élément cartographique sont déplacés.  
  
 Les données de chaque élément cartographique sont déterminées par le format des données spatiales de la source externe. Par exemple, lorsqu'une requête spécifie des données spatiales d'une base de données SQL Server, chaque ligne du jeu de résultats peut contenir plusieurs jeux de coordonnées de points, de lignes ou de polygones. Tous les éléments cartographiques définis par une ligne unique dans le jeu de résultats sont traités comme une unité. Si vous souhaitez modifier l'affichage de jeux spécifiques de coordonnées, vous devez effectuer l'une des opérations suivantes :  
  
-   Modifiez la requête pour retourner le jeu de coordonnées sous forme de lignes séparées dans le jeu de résultats.  
  
-   Sélectionnez les éléments cartographiques à modifier et définissez les propriétés correspondantes des points, lignes ou polygones incorporés en remplaçant les propriétés d'affichage par défaut pour le type de couche correspondant.  
  
## <a name="my-layer-that-uses-spatial-data-from-an-esri-shapefile-always-has-embedded-data"></a>Ma couche utilise des données spatiales d'un fichier de forme ESRI et elle a toujours des données incorporées.  
 Pour garantir que les rapports contenant des cartes peuvent s'exécuter sur un serveur de rapports, les fichiers de forme ESRI doivent être disponibles en tant que ressources sur le serveur de rapports. Si vous ajoutez une couche à une carte et que vous spécifiez un fichier de forme situé sur votre système de fichiers local, les données spatiales sont incorporées automatiquement dans le rapport.  
  
 Pour remplacer les données incorporées par un lien vers le fichier de forme ESRI, vous devez télécharger le fichier .shp et son fichier .dbf correspondant sur le serveur de rapports, puis modifier la source de données spatiales de la couche.  
  
## <a name="i-renamed-a-data-source-or-dataset-to-a-friendly-name-and-now-no-data-appears-in-my-map"></a>J'ai renommé une source de données ou un dataset avec un nom convivial et maintenant aucunes données ne s'affichent dans ma carte.  
 La définition de rapport n'est pas mise à jour automatiquement lorsque vous modifiez manuellement le nom d'un élément de rapport.  
  
 Lorsque vous modifiez le nom d'un dataset, toutes les régions de données ou couches faisant référence à ce dataset doivent être mises à jour manuellement. Pour relier un tableau matriciel, un graphique ou une jauge à un dataset, sélectionnez l'élément sur l'aire de conception, ouvrez les propriétés de la région de données et sélectionnez le nom du dataset approprié. Pour relier une couche à un dataset, sélectionnez la couche, ouvrez les propriétés de couche et sélectionnez le nom du dataset approprié.  
  
## <a name="my-spatial-data-contains-nulls-and-empty-strings"></a>Mes données spatiales contiennent des chaînes null et vides.  
 Dans les données spatiales pour l'élément de rapport cartographique, les valeurs null sont définies sur zéro (0) et les chaînes vides sont définies sur vierge ("").  
  
 Pour les données spatiales provenant d'une base de données SQL Server, pour modifier ce comportement, vous devez modifier la requête qui retourne les données spatiales.  
  
## <a name="my-map-exceeds-the-maximum-number-of-spatial-elements"></a>Ma carte contient un nombre d'éléments spatiaux supérieur à la limite autorisée.  
 Par défaut, une carte peut avoir 20 000 éléments cartographiques ou 1 000 000 de points. Si votre carte dépasse ces limites, vous pouvez utiliser l'une des approches suivantes :  
  
-   Supprimez une couche.  
  
-   Diminuez la résolution de la carte.  
  
-   Réduisez les coordonnées du point de vue de la carte pour afficher une zone de dimensions réduites.  
  
-   Si les données spatiales proviennent d'un dataset du rapport, définissez un filtre pour limiter les données du dataset. Le filtre doit être défini sur un champ qui n'est pas un type de données spatiales.  
  
-   Si les données spatiales proviennent d'une base de données SQL Server, modifiez la requête pour utiliser des fonctions spatiales de manière à limiter les données à une zone moins étendue.  
  
##  <a name="viewport-center-and-view-issues"></a><a name="Viewport"></a> Problèmes liés au centre d'affichage et à la vue  
 Utilisez cette section pour résoudre des problèmes liés aux options de la fenêtre d'affichage.  
  
## <a name="i-cannot-set-the-center-and-view-on-an-embedded-map-element"></a>Je ne peux pas définir le centre et la vue sur un élément cartographique incorporé.  
 Pour centrer une fenêtre d'affichage sur un élément cartographique spécifique, vous avez dû associer les données spatiales d'une couche à des données analytiques.  
  
## <a name="i-set-the-map-center-and-view-in-my-report-when-i-reopen-the-report-why-isnt-the-map-view-the-same"></a>J'ai défini le centre et la vue cartographique dans mon rapport. Lorsque j'ouvre le rapport à nouveau, pourquoi la vue cartographique n'est-elle pas la même ?  
 Si les informations d'identification de l'utilisateur requises pour lire des données spatiales ne sont pas disponibles pour le rapport lorsque vous l'ouvrez, des données spatiales d'espace réservé sont utilisées. Selon les options de centre et de zoom définies pour le point de vue de la carte, la vue cartographique peut être centrée sur une couche différente.  
  
 Pour recharger les données spatiales et utiliser le centre de vue cartographique enregistré dans le rapport, cliquez avec le bouton droit sur le point de vue de la carte, puis cliquez sur **Recharger**. Après avoir entré les informations d'identification pour la source de données spatiales, la couche charge les données spatiales et la vue cartographique est restaurée.  
  
## <a name="the-center-and-view-for-a-map-layer-option-does-not-work"></a>L'option permettant de définir le centre et la vue d'une couche ne fonctionne pas.  
 Lorsque la fenêtre d'affichage est configurée pour être centrée sur les données spatiales d'une couche spécifique et que le centre de la vue ne semble pas être le centre de la couche, il existe probablement de petits îlots ou des zones inclus dans les données spatiales qui sont trop petits pour être affichés dans la fenêtre d'affichage. Par exemple, les données spatiales d'un pays peuvent inclure de petits îlots ou d'autres petits territoires isolés faisant partie du territoire du pays. La fenêtre d'affichage utilise toutes les données spatiales pour calculer le centre de la couche.  
  
 Pour remplacer des calculs pour la couche, vous pouvez effectuer l'une des opérations suivantes :  
  
-   Spécifiez un centre personnalisé pour la fenêtre d'affichage.  
  
-   Modifiez le niveau de zoom de la fenêtre d'affichage pour éliminer les emplacements que vous ne souhaitez pas inclure.  
  
-   Incorporez les données spatiales dans le rapport et supprimez les emplacements que vous ne souhaitez pas inclure.  
  
##  <a name="layer-issues"></a><a name="Layers"></a> Problèmes liés aux couches  
 Utilisez cette section pour résoudre des problèmes liés aux options de couches.  
  
## <a name="i-do-not-see-one-or-more-layers-in-my-map"></a>Je ne vois pas une ou plusieurs couches dans ma carte.  
 L'affichage d'une couche dans un rapport dépend de la disponibilité des données spatiales, de la relation entre les données spatiales et les données analytiques, du type de données spatiales et du type de couche correspondant, des options de transparence et de visibilité de la couche et de l'ordre de dessin des couches. Si vous ne voyez pas les données d'une couche, vérifiez les options suivantes :  
  
-   **Type de couche et type de données spatiales.** Le type de couche affiche uniquement les données spatiales qui correspondent à ce type. Par exemple, si le type de couche est Point mais que les données spatiales sont de type Ligne, aucunes données ne s'afficheront.  
  
-   **Valeurs des champs de correspondance.** Les valeurs des champs que vous spécifiez pour mettre en relation des données analytiques et des données spatiales doivent identifier chaque élément cartographique de façon unique. Les champs doivent avoir le même type de données. Les valeurs des champs doivent être identiques. Pour plus d'informations, consultez [Problèmes liés à la légende, à l'échelle de couleurs et à l'échelle des distances](#Legend).  
  
-   **Ordre des couches.** L'ordre des couches dans le volet Carte correspond à l'ordre dans lequel les couches sont dessinées dans le convertisseur de rapport. Les données spatiales des couches dessinées en premier peuvent être remplacées par celles de couches dessinées ultérieurement. Les couches figurant en haut de la liste sont dessinées en premier. Lorsque vous changez l'ordre des couches dans la liste, vous modifiez l'ordre de dessin des couches.  
  
-   **Transparence.** Vous pouvez spécifier indépendamment la transparence pour chaque couche. Les valeurs de transparence par défaut varient en fonction de la façon dont la couche a été ajoutée. Une transparence de 0 % signifie que la couche est opaque et qu'aucunes autres données de couches ne transparaîtront à travers celle-ci. Pour permettre à d'autres données de transparaître à travers une couche existante, définissez la valeur sur un pourcentage plus élevé qui vous donnera l'effet souhaité.  
  
-   **Visibilité.** Les valeurs de visibilité d'une couche peuvent être **Visible**, **Masquée** ou **En fonction du zoom**, selon le niveau de zoom du point de vue de la carte. Les plages maximale et minimale pour le niveau de zoom peuvent également être spécifiées. La visibilité peut être basée sur une expression qui renvoie l'une de ces valeurs.  
  
    > [!TIP]  
    >  Vous pouvez activer/désactiver la visibilité de chaque couche dans le volet Carte. Lors de la conception de chaque couche, désactivez toutes les autres couches pour déterminer si le problème est lié à une couche individuelle ou à des problèmes de transparence entre les couches.  
  
## <a name="i-set-a-filter-on-the-map-layer-and-it-has-no-effect"></a>J'ai défini un filtre sur la couche et il n'a aucun effet.  
 Pour filtrer des données d'une couche, le type de données de l'expression de filtre doit être spécifié. Vérifiez que vous avez spécifié le type de données sous-jacent correct afin que l'équation de filtre évalue correctement la condition spécifiée. Pour plus d’informations, consultez [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
##  <a name="legend-color-scale-and-rule-issues"></a><a name="Legend"></a> Problèmes liés à la légende, à l'échelle de couleurs et aux règles  
 Utilisez cette section pour résoudre des problèmes liés aux règles et aux options de légende et d'échelle de couleurs.  
  
## <a name="how-do-i-control-the-values-in-the-map-legend"></a>Comment puis-je contrôler les valeurs de la légende de carte ?  
 Les valeurs de la légende sont déterminées automatiquement à partir des règles pour les types d'éléments cartographiques que vous spécifiez pour chaque couche et des règles de distribution que vous spécifiez pour la légende.  
  
 Par défaut, tous les éléments générés par toutes les règles s'affichent dans la première légende. Les valeurs de toutes les règles de polygones, de lignes et de points pour chaque couche contribuent à la plage de légendes combinée. Pour afficher des éléments dans des légendes différentes, vous devez tout d'abord créer plusieurs légendes, puis, pour chaque règle, spécifier dans quelle légende vous souhaitez afficher les éléments connexes.  
  
 Pour associer une règle à une légende spécifique, ouvrez les propriétés de la règle, puis dans la page Légende, sélectionnez le nom de la légende à utiliser. Pour supprimer des éléments d'une légende, dans les options de légende, sélectionnez la ligne vierge correspondant au nom de la légende. Si vous renommez des éléments de légende dans le rapport, vous devez associer manuellement chaque couche à l'élément de légende approprié.  
  
 Pour contrôler le titre et le contenu de chaque légende, utilisez les propriétés Légende de la règle. Vous pouvez spécifier le nombre de divisions à créer, modifier les calculs qui attribuent des valeurs à chaque division, définir des valeurs de plages minimale et maximale et modifier la mise en forme du texte de la légende.  
  
 Pour plus d’informations, consultez [Modifier les légendes de carte, l’échelle de couleurs et les règles associées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
## <a name="the-rules-that-i-set-do-not-give-the-results-that-i-expect"></a>Les règles que j'ai définies ne donnent pas les résultats attendus.  
 Les règles s'appliquent aux données analytiques associées aux éléments cartographiques d'une couche. Utilisez la liste suivante pour identifier des problèmes liés aux règles de couleur, de taille, de largeur et de type de marqueur :  
  
-   La priorité en matière d'application d'un style à chaque élément cartographique (polygone, ligne, point) est, de la priorité la plus basse à la plus élevée : propriétés de la couche ; propriétés des éléments cartographiques pour tous les éléments cartographiques de la couche ; règles que vous spécifiez ; enfin, pour les éléments cartographiques incorporés pour lesquels vous sélectionnez l'option de remplacement, les valeurs que vous spécifiez. Une fois l'option de remplacement sélectionnée pour un élément incorporé, les règles ne s'appliquent plus, même si vous restaurez ultérieurement les valeurs d'origine.  
  
-   Problèmes liés aux champs de correspondance. Les champs de correspondance permettent la liaison de données entre des éléments cartographiques et des données analytiques. Les champs de données spatiales et de données analytiques correspondant aux champs de correspondance doivent avoir le même type de données et le même format. Si le champ de correspondance ne correspond pas exactement aux données spatiales et aux données analytiques, la règle n'a aucun effet. Par exemple, si le champ de correspondance des données spatiales contient des espaces supplémentaires ou une ponctuation supplémentaire par rapport au champ de correspondance des données analytiques, aucune correspondance ne sera assurée.  
  
-   Pour plus d’informations, consultez [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="what-is-the-value-nan-on-the-color-scale"></a>Qu'est-ce que la valeur NaN sur l'échelle de couleurs ?  
 **NaN** signifie Non numérique (Not a Number). Les valeurs d'échelles de couleurs sont supposées être numériques. Vérifiez les paramètres de distribution et la valeur du texte de légende pour les règles associées à l'échelle de couleurs. Si vous avez créé des plages de distribution personnalisées, vérifiez que vous avez spécifié la limite inférieure sur la première plage et la limite supérieure sur la dernière plage.  
  
## <a name="my-color-scale-does-not-appear-when-i-run-the-report"></a>Mon échelle de couleurs ne s'affiche pas lorsque j'exécute le rapport.  
 L'échelle de couleurs affiche des informations à l'intention de l'utilisateur lorsqu'une couche spécifie des règles de couleur pour les polygones, les lignes ou les points de l'ensemble de la couche ou pour les éléments cartographiques incorporés. Si aucun élément cartographique ne spécifie une règle de couleur, ou si les règles de couleur sont spécifiées à l'aide d'une légende plutôt que de l'échelle de couleurs, l'échelle de couleurs ne s'affiche pas dans le rapport rendu.  
  
 Pour afficher l'échelle de couleurs, spécifiez des règles de couleur pour une couche ou un élément cartographique incorporé. Pour plus d’informations, consultez [Modifier les légendes de carte, l’échelle de couleurs et les règles associées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
##  <a name="tile-issues"></a><a name="Tile"></a> Problèmes liés aux mosaïques  
 Utilisez cette section pour résoudre des problèmes liés aux options d'arrière-plan de mosaïques.  
  
## <a name="i-cannot-see-the-bing-maps-tile-background"></a>Je ne peux pas voir l'arrière-plan de mosaïques Bing.  
 Les paramètres suivants déterminent si un arrière-plan de mosaïques Bing Maps est affiché dans l'aperçu local ou sur un rapport exécuté à partir du serveur de rapports :  
  
-   La couche de mosaïques doit exister. Dans l'Assistant Carte ou Couche, sélectionnez **Ajouter un arrière-plan Bing Maps pour cette vue cartographique**. Cette option ajoute une couche de mosaïques pour le centre d'affichage et le niveau de zoom du point de vue de la carte actif. Vous pouvez également ajouter une couche de mosaïques à partir de la barre d'outils du volet Carte.  
  
-   Le système de coordonnées de la carte pour la fenêtre d'affichage doit être **Géographique** et non **Planaire**.  
  
-   La projection cartographique doit être **Mercator**.  
  
-   Pour l'aperçu local, vous devez avoir accès à Internet. Pour un rapport exécuté à partir du serveur de rapports, celui-ci doit être configuré pour prendre en charge l'arrière-plan de mosaïques. Pour plus d'informations, consultez [Planifier un rapport cartographique](plan-a-map-report-report-builder-and-ssrs.md).  
  
 Pour plus d’informations sur l’ajout d’une couche de vignettes, consultez [Ajouter, modifier ou supprimer une carte ou une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
## <a name="how-do-i-control-the-text-on-a-tile-layer"></a>Comment puis-je contrôler le texte sur une couche de mosaïques ?  
 Les vues **Route** et **Hybride** incluent toutes deux du texte. Le texte fait partie des mosaïques provenant des services Web Bing Maps.  
  
 Pour inclure une couche de mosaïques sans texte, sélectionnez la vue **Aérien** .  
  
##  <a name="tooltip-and-label-issues"></a><a name="Tooltip"></a> Problèmes liés aux info-bulles et aux étiquettes  
 Utilisez cette section pour résoudre des problèmes liés aux options d'étiquettes ou d'info-bulles.  
  
## <a name="i-get-an-expression-error-about-dataset-scope-when-i-set-a-label-or-tooltip-to-an-expression"></a>J'obtiens une erreur d'expression concernant l'étendue du dataset lorsque je définis une étiquette ou une info-bulle sur une expression.  
 Si vos données spatiales proviennent d'une bibliothèque de cartes ou d'un fichier de forme ESRI, les données associées ne font pas partie d'un dataset du rapport. Vous ne pouvez pas utiliser la syntaxe d'expression pour une référence de champ de dataset pour spécifier ces données pour une étiquette ou une info-bulle.  
  
 Pour spécifier des données liées à des données spatiales qui ne font pas partie d'un dataset du rapport, vous devez utiliser le symbole #, suivi d'une étiquette qui spécifie le nom des données.  
  
## <a name="see-also"></a>Voir aussi  
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Résolution des problèmes liés au Générateur de rapports](/previous-versions/sql/2014/reporting-services/troubleshoot-report-builder?view=sql-server-2014&preserve-view=true)  
  
  
