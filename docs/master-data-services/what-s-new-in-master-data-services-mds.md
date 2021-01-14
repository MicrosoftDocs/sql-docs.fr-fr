---
title: Nouveautés
description: Découvrez les modifications, les améliorations et les mises à jour de la version SQL Server 2019 de Master Data Services.
ms.custom: ''
ms.date: 07/08/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ad530f60-d480-4457-ba7a-93a10c8a1695
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 7e731d14967d808aa57f3fa0e001e5e593238ffd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170431"
---
# <a name="what39s-new-in-master-data-services-mds"></a>Nouveautés de Master Data Services (MDS)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cette rubrique résume les changements et les mises à jour de la version [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. 
  
 Si vous souhaitez une vue d’ensemble de l’organisation des données dans [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consultez l’article [Vue d’ensemble de Master Data Services](../master-data-services/master-data-services-overview-mds.md). 
  
 **Pour installer Master Data Services, configurer la base de données et le site web, et déployer les exemples de modèles, consultez l’article** [Vue d’ensemble de Master Data Services (MDS)](../master-data-services/master-data-services-overview-mds.md).  
  
 **Télécharger**  
  
-   Pour télécharger [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], accédez au  **[Centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** .  
  
-   Vous avez un compte Azure ?  Cliquez **[ici](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview)** pour lancer une machine virtuelle déjà équipée de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] .  
  
##  <a name="improved-performance"></a>Performances améliorées  
  
 L’amélioration des performances vous permet de créer des modèles plus volumineux, de charger les données de façon plus efficace et d’obtenir de meilleures performances globales. Cette amélioration des performances concerne aussi le complément pour Microsoft Excel, qui a été amélioré pour réduire le temps de chargement des données et traiter des entités plus volumineuses.  
  
 Pour plus d’informations sur le complément pour Microsoft Excel, consultez[Complément Master Data Services pour Microsoft Excel](../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md).  
  
 Les améliorations touchent également les fonctionnalités suivantes.  
  
-   La compression des données s’effectue au niveau de l’entité, fonctionnalité activée par défaut. Quand la compression des données est activée, l’ensemble des tables et des index liés à une entité sont compressés au moyen de la compression SQL au niveau des lignes. Cela réduit considérablement les E/S disque pendant la lecture ou la mise à jour des données de référence, en particulier quand les données de référence contiennent plusieurs millions de lignes et/ou un grand nombre de colonnes de valeur NULL.  
  
     Sachant que le processeur est un peu plus sollicité du côté du moteur SQL Server, si le serveur est tributaire du processeur, vous pouvez désactiver la compression de données en modifiant l’entité.  
  
     Pour plus d’informations, consultez les articles [Créer une entité (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md) et [Compression de données](../relational-databases/data-compression/data-compression.md).  
  
-   La fonctionnalité IIS de compression du contenu dynamique est activée par défaut. Cela réduit considérablement la taille de la réponse xml et épargne des E/S réseau, mais le processeur est davantage sollicité. Si le serveur est tributaire du processeur, vous pouvez désactiver la compression de données en ajoutant le paramètre suivant au fichier Web.config de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
    ```  
    <configuration>  
       \<system.webServer>  
          <urlCompression doStaticCompression="true" doDynamicCompression="false " />  
       \</system.webServer>  
    </configuration>  
  
    ```  
  
     Pour plus d’informations, consultez [URL Compression](https://www.iis.net/configreference/system.webserver/urlcompression)(Compression d’URL)  
  
-   Les nouveaux travaux SQL Server Agent suivants assurent la maintenance des index et des journaux.  
  
    -   MDS_MDM_Sample_Index_Maintenace  
  
    -   MDS_MDM_Sample_Log_Maintenace  
  
 Par défaut, le travail MDS_MDM_Sample_Index_Maintenance s’exécute toutes les semaines. Vous pouvez en modifier la planification. Vous pouvez aussi exécuter le travail manuellement à tout moment en utilisant la procédure stockée udpDefragmentation. Il est recommandé d’exécuter cette procédure stockée chaque fois qu’un volume important de données de référence est inséré ou mis à jour, ou après qu’une nouvelle version est créée à partir de la version existante.  
  
 Un index présentant une fragmentation de plus de 30 % est régénéré en ligne. À cette occasion, les performances sont affectées quand des opérations CRUD sont effectuées sur la même table. Si la dégradation des performances vous pose problème, il est recommandé d’exécuter la procédure stockée en dehors des heures ouvrées. Pour plus d'informations sur la fragmentation des index, consultez [Reorganize and Rebuild Indexes](../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Pour plus d’informations, consultez ce billet sur le blog Master Data Services, [Performance and Scale Improvement in SQL Server 2016](https://techcommunity.microsoft.com/t5/sql-server-integration-services/performance-and-scale-improvement-in-sql-server-2016/ba-p/388225)(Amélioration des performances et de la mise à l’échelle dans SQL Server 2016).  
  
##  <a name="improved-security"></a>Sécurité renforcée  
  
 La nouvelle autorisation de la fonction Super utilisateur confère à un utilisateur ou à un groupe les mêmes autorisations que celles de l’administrateur du serveur dans la version précédente de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. L’autorisation Super utilisateur peut être attribuée à plusieurs utilisateurs et groupes. Dans la version précédente, l’utilisateur qui installait à l’origine [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] était l’administrateur du serveur, et il était difficile de transférer cette autorisation à un autre utilisateur ou groupe. Pour plus d’informations, consultez [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
 Un utilisateur peut désormais se voir attribuer explicitement l’autorisation d’administrateur au niveau du modèle. Cela signifie que si des autorisations sont attribuées par la suite à l’utilisateur dans la sous-arborescence de modèle, par exemple au niveau de l’entité, il ne perd pas cette autorisation d’administrateur.  
  
 Dans cette version de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], nous proposons d’autres niveaux d’autorisations en introduisant de nouvelles autorisations (lecture, création, mise à jour et suppression). Par exemple, un utilisateur qui dispose seulement de l’autorisation de mise à jour peut désormais mettre à jour les données de référence sans créer ni supprimer les données. Quand vous attribuez à un utilisateur l’autorisation de création, de mise à jour ou de suppression, il bénéficie automatiquement de l’autorisation de lecture. Vous pouvez aussi combiner les autorisations de lecture, de création, de mise à jour et de suppression.  
  
 Quand vous procédez à une mise à niveau vers [!INCLUDE[ssSQL15](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], les anciennes autorisations sont transformées en nouvelles autorisations, comme indiqué dans le tableau suivant.  
  
|Autorisation dans la version précédente|Nouvelle autorisation|  
|------------------------------------|--------------------|  
|L’utilisateur qui installe à l’origine [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] dispose de l’autorisation d’administrateur du serveur.|L’utilisateur dispose de l’autorisation de la fonction Super utilisateur.|  
|L’utilisation dispose d’autorisations de mise à jour au niveau du modèle et aucune autorisation dans la sous-arborescence de modèle. Il est donc implicitement un administrateur de modèle.|L’utilisateur dispose d’autorisations d’administrateur explicites au niveau du modèle.|  
|L’utilisateur dispose d’autorisations de lecture seule.|L’utilisateur dispose d’autorisations d’accès en lecture.|  
|L’utilisateur dispose d’autorisations de mise à jour.|L’utilisateur dispose de l’ensemble des quatre autorisations d’accès : création, mise à jour, suppression et lecture.|  
|L’utilisateur dispose d’autorisations de refus.|L’utilisateur dispose d’autorisations de refus.|  
  
 Pour plus d’informations sur les autorisations, consultez l’article [Sécurité (Master Data Services)](../master-data-services/security-master-data-services.md).  
  
##  <a name="improved-transaction-log-maintenance"></a>Gestion améliorée des journaux des transactions  
  
 Vous pouvez désormais nettoyer les journaux des transactions à intervalles prédéterminés ou selon une planification, en utilisant les paramètres système et au niveau du modèle. Dans un système MDS où les modifications de données et les processus ETL sont nombreux, ces tables peuvent croître de manière exponentielle et occasionner une dégradation des performances et des problèmes d’espace de stockage.  
  
 Les types de données suivants peuvent être supprimés des journaux.  
  
-   Historique des transactions antérieur à un nombre de jours spécifié.  
  
-   Historique des problèmes de validation antérieur à un nombre de jours spécifié.  
  
-   Lots intermédiaires dont l’exécution est antérieure à un nombre de jours spécifié.  
  
 Vous pouvez configurer la fréquence de suppression des données des journaux des transactions, en utilisant les paramètres système et au niveau du modèle. Pour plus d’informations, consultez les articles [Paramètres système (Master Data Services)](../master-data-services/system-settings-master-data-services.md) et [Créer un modèle (Master Data Services)](../master-data-services/create-a-model-master-data-services.md). Pour plus d’informations sur les transactions, consultez [Transactions &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
 Le travail SQL Server Agent, MDS_MDM_Sample_Log_Maintenace, déclenche le nettoyage des journaux des transactions et s’exécute toutes les nuits. Vous pouvez utiliser SQL Server Agent pour modifier la planification de ce travail.  
  
 Vous pouvez aussi appeler des procédures stockées pour nettoyer les journaux des transactions. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
## <a name="improved-troubleshooting"></a>Résolution des problèmes améliorée  
  
 Des fonctionnalités ont été ajoutées à [!INCLUDE[ssSQL15](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]pour améliorer le débogage et faciliter la résolution des problèmes. Pour plus d’informations, consultez l’article [Suivi (Master Data Services)](../master-data-services/tracing-master-data-services.md).  
  
## <a name="improved-manageability"></a>Facilité de gestion accrue  
  
 Les améliorations sur le plan de la facilité de gestion contribuent à réduire les coûts de maintenance et ont un impact positif sur votre retour sur investissement (ROI). Ces améliorations concernent notamment la gestion des journaux des transactions, la sécurité, ainsi que les nouvelles fonctionnalités suivantes.  
  
-   Utilisation de noms d’attributs de plus de 50 caractères.  
  
-   Changement de nom et masquage des attributs Name et Code.  
  
 Pour plus d'informations, consultez les rubriques ci-dessous.  
  
-   [Modèles &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)  
  
-   [Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
-   [Transactions &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md)  
  
-   [Sécurité &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md)  

## <a name="business-rule-improvements"></a>Améliorations des règles d’entreprise
 **Gérer les règles d’entreprise (complément MDS pour Excel)**  
  
 Dans le complément Master Data Services pour Excel, vous pouvez gérer les règles d’entreprise, notamment en créer et en modifier. Les règles d’entreprise servent à valider les données.  
 
 **Business Rules Extension**  
  
 Vous pouvez appliquer des scripts SQL définis par l’utilisateur en tant qu’extension des conditions et des actions de règles d’entreprise. Les fonctions SQL peuvent être utilisées en tant que condition. Les procédures stockées SQL peuvent être utilisées en tant qu’action. Pour plus d’informations, consultez l’article [Extension de règles d’entreprise (Master Data Services)](../master-data-services/business-rules-extension-master-data-services.md). 
 
 **Expérience de gestion des règles d’entreprise repensée**  
  
 L’expérience de gestion des règles d’entreprise dans MDS a été complètement repensée en vue de l’améliorer. Pour plus d’informations sur cette fonctionnalité, consultez l’article [Règles d’entreprise (Master Data Services)](../master-data-services/business-rules-master-data-services.md).  
  
 **Fonctionnalités de gestion des règles d’entreprise retirées du complément MDS pour Excel**  
  
 Les fonctionnalités de gestion des règles d’entreprise ont été retirées du complément MDS pour Excel, car nous avons repensé l’expérience.    

 **Nouvelles conditions de règle d’entreprise**  
  
 Sept nouvelles conditions de règle d’entreprise ont été ajoutées pour proposer un ensemble complet de conditions. Pour plus d’informations, consultez l’article [Conditions de règle d’entreprise (Master Data Services)](../master-data-services/business-rule-conditions-master-data-services.md).  

## <a name="derived-hierarchy-improvements"></a>Améliorations de la hiérarchie dérivée

 **Relations plusieurs-à-plusieurs dans les hiérarchies dérivées**  
  
 Vous pouvez maintenant créer une hiérarchie dérivée qui affiche les relations plusieurs-à-plusieurs. Une relation plusieurs-à-plusieurs entre deux entités peut être modélisée à travers l’utilisation d’une troisième entité qui fournit un mappage entre les deux. L’entité de mappage est une entité qui possède plusieurs attributs basés sur un domaine qui font référence à d’autres entités.  
  
 Par exemple, l’entité M a un attribut basé sur un domaine qui fait référence à A et un attribut basé sur un domaine qui fait référence à B. Vous pouvez créer une hiérarchie de A à B en utilisant l’entité de mappage.  
  
 Pour plus d’informations, consultez l’article [Afficher les relations plusieurs à plusieurs dans des hiérarchies dérivées (Master Data Services)](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md)  
 
 **Modifier les relations plusieurs-à-plusieurs dans les hiérarchies dérivées**  
  
 Vous pouvez modifier une relation plusieurs-à-plusieurs en modifiant les membres de l’entité de mappage. Pour plus d’informations, consultez [afficher les relations plusieurs-à-plusieurs dans les hiérarchies dérivées &#40;Master Data Services&#41;](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md).  
 
 **Expérience de gestion des hiérarchies dérivées améliorée**  
  
 L’expérience de gestion des hiérarchies dérivées dans MDS a été améliorée. Pour plus d’informations sur cette fonctionnalité, consultez l’article [Créer une hiérarchie dérivée (Master Data Services)](../master-data-services/create-a-derived-hierarchy-master-data-services.md).  
  
 Les fonctionnalités de gestion des règles d’entreprise ont été retirées du complément MDS pour Excel, car nous avons repensé l’expérience.  
 
## <a name="attribute-improvements"></a>Améliorations des attributs   
    
 **Index personnalisés**  
  
 Vous pouvez créer un index non-cluster sur un attribut (index unique) ou sur une liste d’attributs (index composite), dans une entité, afin d’améliorer les performances des requêtes. Pour plus d’informations, consultez [Index personnalisé &#40;Master Data Services&#41;](../master-data-services/custom-index-master-data-services.md).  
 
  **Filtres d’attribut**  
  
 Pour un attribut basé sur un domaine, pour un membre feuille, vous pouvez utiliser un attribut parent de filtre pour limiter les valeurs autorisées pour l’attribut basé sur un domaine. Pour plus d’informations, consultez [Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md).  
 
## <a name="entity-and-member-improvements"></a>Améliorations des entités et des membres 
  
 **Relation de synchronisation d’entités**  
  
 Vous pouvez partager des données d’entité entre différents modèles en créant une relation de synchronisation d’entité. Pour plus d’informations, consultez l’article [Relation de synchronisation d’entités (Master Data Services)](../master-data-services/entity-sync-relationship-master-data-services.md).  
  
 **Purger les membres supprimés de manière réversible**  
  
 Vous pouvez désormais purger (supprimer définitivement) tous les membres supprimés de manière réversible dans une version de modèle. La suppression d’un membre n’a pour effet que de le désactiver (suppression réversible). Pour plus d’informations, consultez l’article [Purge des membres de version (Master Data Services)](../master-data-services/purge-version-members-master-data-services.md).  
 
## <a name="improvements-for-managing-changes"></a>Améliorations de la gestion des modifications 
  
 **Historique de révision de membre**  
  
 Un historique de révision de membre est enregistré quand un membre fait l’objet d’une modification. Vous pouvez restaurer un historique de révision, tout comme vous pouvez afficher et annoter les révisions. À l’aide de la propriété **Nombre de jours de rétention du journal** , vous pouvez spécifier la durée de conservation des données d’historique. Pour plus d’informations, consultez l’article [Historique de révision de membre (Master Data Services)](../master-data-services/member-revision-history-master-data-services.md).  
  
 **Fusionner les conflits**  
  
 Si vous essayez de publier des données qui ont été modifiées par un autre utilisateur, la publication échouera avec une erreur de conflit. Pour résoudre cette erreur, vous pouvez exécuter la fonctionnalité Conflits de fusion et publier à nouveau les modifications. Pour plus d’informations, consultez les articles [Fusionner les conflits (Master Data Services)](../master-data-services/merge-conflicts-master-data-services.md) et [Fusionner les conflits (complément MDS pour Excel)](../master-data-services/microsoft-excel-add-in/merge-conflicts-mds-add-in-for-excel.md).  
  
 **Ensembles de modifications**  
  
 Vous pouvez utiliser des ensembles de modifications pour enregistrer les modifications en attente d’une entité, et vous pouvez afficher et modifier les modifications en attente. Si l’entité exige une approbation des modifications, vous devez enregistrer les modifications en attente dans un ensemble de modifications et les soumettre à l’administrateur pour approbation. Pour plus d’informations, consultez ensembles de modifications [&#40;Master Data Services&#41;](../master-data-services/changesets-master-data-services.md).  
  
 **Gestion des ensembles de modifications et notifications par courrier électronique**  
  
 Dans cette version, vous pouvez désormais afficher et gérer toutes les modifications par modèle et version. Vous pouvez aussi recevoir des notifications par courrier électronique chaque fois que l’état d’un ensemble de modifications change pour une entité qui exige une approbation. Pour plus d’informations, consultez les articles [Gérer les ensembles de modifications (Master Data Services)](../master-data-services/manage-changesets-master-data-services.md) et [Notifications (Master Data Services)](../master-data-services/notifications-master-data-services.md).  
  
 **Afficher et gérer l’historique de révision**  
  
 Vous pouvez afficher et gérer l’historique de révision par entité et par membre. Si vous disposez d’autorisations de mise à jour, vous pouvez restaurer un membre dans sa version précédente. Pour plus d’informations, consultez l’article [Historique de révision de membre (Master Data Services)](../master-data-services/member-revision-history-master-data-services.md).  
 
## <a name="tool-and-sample-improvements"></a>Outil et améliorations des exemples 
  
 **Enregistrer ou ouvrir des fichiers de requête dans le complément MDS pour Excel**  
  
 Dans la page Explorateur d’entité, vous pouvez cliquer sur **Excel** pour enregistrer les fichiers de requête de raccourci. Vous pouvez aussi ouvrir le fichier de requête stocké sur votre ordinateur, dans le complément MDS pour Excel. Le fichier enregistré peut être ouvert à l’aide de l’application QueryOpener. Pour plus d’informations, consultez l’article [Fichiers de requête de raccourci (Complément MDS pour Excel)](../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md).  
  
 Le fichier de requête contient les filtres et les informations de hiérarchie de la page de l’explorateur.  
   
 **Exemples de packages de déploiement de modèle mis à jour**  
  
 Les exemples de packages ont été mis à jour pour prendre en charge de nouveaux scénarios. Pour plus d’informations, consultez l’article [Exemples : packages de déploiement de modèles (Master Data Services)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md).  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
  
## <a name="see-also"></a>Voir aussi  
 [Master Data Services et fonctionnalités de Services de qualité de données pris en charge par les éditions de SQL Server 2016](../master-data-services/master-data-services-and-data-quality-services-features-support.md)  
 [Fonctionnalités Master Data Services déconseillées](../master-data-services/deprecated-master-data-services-features.md)  
 [Fonctionnalités Master Data Services éliminées](../master-data-services/discontinued-master-data-services-features.md)
