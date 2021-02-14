---
description: Afficher les relations plusieurs à plusieurs dans des hiérarchies dérivées (Master Data Services)
title: Afficher les relations plusieurs à plusieurs dans des hiérarchies dérivées
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8b2a9c43-40e0-48f7-a6a9-325beb9f27da
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f550a109d5271a216614d0566ed6f3a4d87be056
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336297"
---
# <a name="show-many-to-many-relationships-in-derived-hierarchies-master-data-services"></a>Afficher les relations plusieurs à plusieurs dans des hiérarchies dérivées (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Les hiérarchies dérivées affichent des relations un à plusieurs et peuvent désormais également afficher des relations plusieurs à plusieurs.  
  
## <a name="many-to-many-m2m-relationships"></a>Relations plusieurs à plusieurs  
 Une relation plusieurs à plusieurs entre deux entités peut être modélisée via l’utilisation d’une troisième entité effectuant un mappage entre les elles :  
  
 ![mds_hierarchies_manytomany](../master-data-services/media/mds-hierarchies-manytomany.png "mds_hierarchies_manytomany")  
  
 Dans l’exemple ci-dessus, il existe une relation plusieurs à plusieurs entre les entités **Employee** et **TrainingClass** , fournie par l’entité de mappage **ClassRegistration**. Un employé peut être inscrit en tant qu’un étudiant dans plusieurs classes, et chaque classe peut contenir plusieurs étudiants.  
  
 Auparavant, les hiérarchies dérivées ne pouvaient pas modéliser des relations plusieurs à plusieurs. Désormais, dans [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], vous pouvez créer une hiérarchie dérivée affichant, par exemple, les étudiants par classe, ou inverser la relation et afficher les classes regroupées par étudiant.  
  
 Accédez d’abord à la page de gestion des hiérarchies dérivées, puis créez une hiérarchie dérivée :  
  
 ![mds_hierarchies_add_derived_hierarchy](../master-data-services/media/mds-hierarchies-add-derived-hierarchy.png "mds_hierarchies_add_derived_hierarchy")  
  
 Ensuite, ajoutez des niveaux à la nouvelle hiérarchie dérivée, en commençant par le bas. Dans cet exemple, nous souhaitons afficher étudiants (employés) regroupés par classe. L’entité **mployee** entité est par conséquent le niveau feuille de la hiérarchie, et est ajoutée en premier :  
  
 ![mds_hierarchies_edit_derived_hierarchy_one](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-one.PNG "mds_hierarchies_edit_derived_hierarchy_one")  
  
 Dans la capture d’écran ci-dessus, notez que l’entité **employé** apparaît sous **Niveaux actuels** , au milieu, comme le seul niveau. L’ **aperçu** de la hiérarchie dérivée à droite affiche simplement la liste de tous les membres de l’entité **Employee** . La section **Entités et hiérarchies disponibles** à gauche montre les niveaux qui peuvent être ajoutés en plus du niveau supérieur en cours (**Employee**). La plupart d’entre eux sont des attributs basés sur un domaine (DBA) sur l’entité **Employee** , y compris le DBA **Department** .  
  
 À partir de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], un nouveau type de niveau modélise les relations plusieurs à plusieurs, par exemple : **Class (mappé via ClassRegistration.Student)**. Le nom du niveau est plus détaillé que les autres afin de refléter les informations supplémentaires nécessaires pour décrire clairement la relation de mappage. Faites glisser ce niveau vers le niveau **Employee** dans la section **Niveaux actuels** :  
  
 ![mds_hierarchies_edit_derived_hierarchy_two](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-two.PNG "mds_hierarchies_edit_derived_hierarchy_two")  
  
 L’aperçu affiche à présent les employés regroupés en fonction des classes de formation auxquelles ils sont inscrits. Dans la mesure où il s’agit d’une relation plusieurs à plusieurs, chaque membre enfant peut avoir plusieurs parents. Dans l’exemple ci-dessus, l’employé **6 {Hillman, Reinout N}** est enregistré en tant qu’étudiant dans deux classes, **1 {Master Data Services 101}** et **4 {Career-Limiting Moves}**.  
  
 Cette relation de mappage peut également être affichée de façon inversée, en regroupant les classes par étudiant :  
  
 ![mds_hierarchies_available_entities_and_hierarchies](../master-data-services/media/mds-hierarchies-available-entities-and-hierarchies.PNG "mds_hierarchies_available_entities_and_hierarchies")  
  
 Là encore, nous voyons comment un enfant peut apparaître sous plusieurs parents : la classe de formation **1 {Master Data Services 101}** s’affiche sous **6 {Hillman, Reinout N}** et **40 {Ford, Jeffrey L}**.  
  
 Les membres de l’entité de mappage **ClassRegistration** n’apparaissent nulle part dans la hiérarchie dérivée. Ils servent simplement à définir les relations entre les membres parents et enfants dans la hiérarchie.  
  
 Vous pouvez modifier la relation plusieurs à plusieurs en modifiant les membres de l’entité qui mappe de l’une des manières suivantes. La relation plusieurs à plusieurs est en lecture seule dans la page de **l’explorateur Hiérarchie dérivée** .  
  
-   Modifiez les membres de l’entité de mappage dans la page de l’ **explorateur d’entités** en utilisant le complément Master Data Services pour Excel, ou en procédant à une mise en lots des données.  
  
-   Glissez-déplacez les nœuds enfants entre les parents dans la page de l’ **explorateur Hiérarchie dérivée**.  
  
     Cette méthode modifie les membres existants quand c’est possible, et ajoute de nouveaux membres si nécessaire. Les membres existants ne sont pas supprimés.  
  
     Par exemple, avec l’entité de mappage ClassRegistration, lorsque vous déplacez un étudiant vers le nœud inutilisé, la valeur d’attribut de classe du membre d’entité de mappage correspondant est modifiée sur Null, et le membre n’est pas supprimé. Inversement, lorsque vous déplacez un étudiant à partir du nœud inutilisé vers une classe, s’il existe un membre de mappage correspondant à l’étudiant pour lequel la classe est Null, ce membre est modifié en remplaçant la classe Null par le nouveau parent. Si aucun membre n’est trouvé, un membre est ajouté.  
  
     Ce processus évite la suppression de membres pour empêcher toute suppression involontaire d’autres données utilisateur, par exemple, si l’entité de mappage contient des attributs autres que les deux attributs définissant la relation parent-enfant. Les utilisateurs doivent opérer explicitement les suppressions directement sur l’entité qui effectue le mappage.  
  
 Le nouveau niveau plusieurs à plusieurs peut apparaître n’importe où dans une hiérarchie dérivée dont le niveau d’attribut basé sur un domaine (DBA) est autorisé. Un niveau plusieurs à plusieurs peut se trouver en haut, comme dans les exemples ci-dessus. Il peut être au-dessus ou au-dessous d’un niveau DBA, y compris récursif. Il peut être sous un niveau supérieur de la hiérarchie explicite (déconseillé). Des relations plusieurs à plusieurs peuvent être chaînées dans une même hiérarchie dérivée.  
  
 Des niveaux plusieurs à plusieurs peuvent être masqués, tout comme les autres niveaux de la hiérarchie dérivée.  
   
### <a name="m2m-relationship-in-sample-model"></a><a name="M2MSample"></a> Relation plusieurs à plusieurs dans l’exemple de modèle  
La hiérarchie dérivée Region Climate de l’exemple de modèle Customer fournie avec [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]vous permet de visualiser une démonstration d’une relation plusieurs à plusieurs.   
  
Comme illustré dans l’image ci-dessous, le nom du niveau qui modélise cette relation est ![mds_Number1](../master-data-services/media/mds-number1.png)**Climate (mapped via RegionClimate.Region)**. La section ![mds_Number2](../master-data-services/media/mds-number2.png)**Aperçu** montre les régions regroupées selon les types de climats auxquels elles sont associées. Il s’agit d’une relation plusieurs à plusieurs car certaines régions (membres enfants) sont associées à plusieurs climats (parents). Par exemple, ![mds_Number3](../master-data-services/media/mds-number3.png)**APCR {Asia Pacific}** est associé à ![mds_Number4](../master-data-services/media/mds-number4.png)**A {Tropical}** et ![mds_Number5](../master-data-services/media/mds-number5.png)**B {Dry}**.  
  
![mds_M2MRelationship_Example_CustomerModel](../master-data-services/media/mds-m2mrelationship-example-customermodel.png)  
  
Pour obtenir des instructions sur le déploiement de l’exemple de modèle Customer et les autres exemples de modèles fournis avec [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consultez [Déploiement des exemples de modèles et de données](~/master-data-services/sql-server-samples-model-deployment-packages-mds.md).   
  
## <a name="one-many-relationship"></a>Relation un à plusieurs.  
 Un membre d’une hiérarchie dérivée peut être le parent de nombreux membres enfants, mais il ne peut généralement pas avoir plus d’un parent (pour les exceptions, voir [Sécurité des membres](#bkmk_member_security)). Par exemple, il existe deux entités, Employee et Department, dans lesquelles chaque employé appartient à un seul service. Cette relation est modélisée en ajoutant à l’entité Employee un attribut basé sur un domaine (DBA) qui fait référence à l’entité du service :  
  
 ![mds_hierarchies_onetomany](../master-data-services/media/mds-hierarchies-onetomany.png "mds_hierarchies_onetomany")  
  
 Il s’agit d’une relation un à plusieurs, car chaque employé fait partie d’un seul service, et chaque service peut compter plusieurs employés. Il est possible de créer une hiérarchie dérivée qui affiche les employés regroupés par service :  
  
 ![mds_hierarchies_dh_screenshot](../master-data-services/media/mds-hierarchies-dh-screenshot.png "mds_hierarchies_dh_screenshot")  
  
##  <a name="member-security"></a><a name="bkmk_member_security"></a> Sécurité des membres  
 Une hiérarchie permettant une duplication des membres (permettant à un membre d’avoir plusieurs parents) ne peut pas être utilisée pour affecter des autorisations de sécurité de membre. Par exemple :  
  
-   Une hiérarchie dérivée qui n’ancre pas les récursions Null (chaque membre au niveau récursif apparaît sous la racine et sous son parent récursif).  
  
-   Une hiérarchie dérivée récursive avec un niveau au-dessus du niveau récursif (chaque membre du niveau récursif apparaît sous son parent non récursif et son parent récursif).  
  
-   Une hiérarchie dérivée avec un niveau plusieurs à plusieurs (un enfant peut être mappé de nombreux parents).  
  
## <a name="collections"></a>Collections  
 Les Hiérarchies explicites et collections sont déconseillées. La procédure stockée de conversion (udpConvertCollectionAndConsolidatedMembersToLeaf) convertit des membres de la collection en membres feuille, et crée des hiérarchies dérivées plusieurs à plusieurs pour capturer les informations d’appartenance.  
  
## <a name="see-also"></a>Voir aussi  
 [Hiérarchies dérivées &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
