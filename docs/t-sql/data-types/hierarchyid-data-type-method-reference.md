---
description: Référence de méthodes de type de données hierarchyid
title: hierarchyid (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- hierarchyid
- hierarchyid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Hierarchy data type
- hierarchyid data type
ms.assetid: 69b756e0-a1df-45b3-8a24-6ded8658aefe
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b11771fe7c2e73854a2214e7218cff1bfda1250f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165145"
---
# <a name="hierarchyid-data-type-method-reference"></a>Référence de méthodes de type de données hierarchyid
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Le type de données **hierarchyid** est un type de données système de longueur variable. Utilisez **hierarchyid** pour représenter une position dans une hiérarchie. Une colonne de type **hierarchyid** ne représente pas automatiquement une arborescence. Il appartient à l'application de générer et d'assigner des valeurs **hierarchyid** de telle façon que la relation voulue entre les lignes soit reflétée dans les valeurs.
  
Une valeur du type de données **hierarchyid** représente une position dans une hiérarchie d'arborescence. Les valeurs de **hierarchyid** ont les propriétés suivantes :
  
-   Extrêmement compact  
     Le nombre moyen de bits nécessaires pour représenter un nœud dans une arborescence avec *n* nœuds dépend de la sortance moyenne (nombre moyen d’enfants d’un nœud). Pour les petites sortances (de 0 à 7), la taille est d’environ 6\*logA *n* bits, où A est la sortance moyenne. Un nœud dans une hiérarchie d'organisation de 100 000 personnes avec une sortance moyenne de 6 niveaux prend approximativement 38 bits. Ce chiffre est arrondi à 40 bits, ou 5 octets, pour le stockage.  
-   La comparaison est effectuée dans l'ordre à profondeur prioritaire  
     Étant donné deux valeurs **hierarchyid****a** et **b**, **a<b** signifie que a se situe avant b dans un parcours à profondeur prioritaire de l’arborescence. Les index sur les types de données **hierarchyid** sont dans l’ordre à profondeur prioritaire, et les nœuds proches les uns des autres dans un parcours à profondeur prioritaire sont stockés les uns à côté des autres. Par exemple, les enfants d'un enregistrement sont stockés à côté de cet enregistrement. Pour plus d’informations, consultez [Données hiérarchiques &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md).  
-   Prise en charge des insertions et suppressions arbitraires  
     En utilisant la méthode [GetDescendant](../../t-sql/data-types/getdescendant-database-engine.md) , il est toujours possible de générer un frère à droite d'un nœud donné, à gauche d'un nœud donné, ou entre les deux frères. La propriété de comparaison est maintenue lorsqu'un nombre arbitraire de nœuds est inséré ou supprimé dans la hiérarchie. La plupart des insertions et suppressions préservent la propriété de compacité. Toutefois, les insertions entre deux nœuds produiront des valeurs hierarchyid avec une représentation légèrement moins compacte.  
-   L’encodage utilisé dans le type **hierarchyid** est limité à 892 octets. Par conséquent, les nœuds qui ont trop de niveaux dans leur représentation pour s’adapter à 892 octets ne peuvent pas être représentés par le type **hierarchyid**.  
  
Le type **hierarchyid** est disponible pour les clients CLR comme le type de données **SqlHierarchyId**.
  
## <a name="remarks"></a>Remarques  
Le type **hierarchyid** encode logiquement les informations sur un nœud unique dans une arborescence hiérarchique en encodant le chemin de la racine de l’arborescence au nœud. Un tel chemin d'accès est représenté logiquement comme une séquence d'étiquettes de nœud de tous les enfants visités après la racine. Une barre oblique démarre la représentation et un chemin d'accès qui visite uniquement la racine est représenté par une barre oblique unique. Pour les niveaux sous la racine, chaque étiquette est encodée comme une séquence d'entiers séparés par des points. La comparaison entre les enfants est effectuée en comparant les séquences d'entiers séparés par des points dans le classement du dictionnaire. Chaque niveau est suivi d'une barre oblique. Par conséquent, une barre oblique sépare les parents de leurs enfants. Par exemple, les éléments suivants sont des chemins de **hierarchyid** valides de longueurs 1, 2, 2, 3, sur 3 niveaux respectivement :
  
-   /  
  
-   /1/  
  
-   /0.3.-7/  
  
-   /1/3/  
  
-   /0.1/0.2/  
  
Les nœuds peuvent être insérés à tout emplacement. Les nœuds insérés après **/1/2/** mais avant **/1/3/** peuvent être représentés comme **/1/2.5/**. Les nœuds insérés avant 0 ont comme représentation logique un nombre négatif. Par exemple, un nœud placé avant **/1/1/** peut être représenté sous la forme **/1/-1/**. Les nœuds ne peuvent pas avoir de zéros non significatifs. Par exemple, **/1/1.1/** est valide, mais **/1/1.01/** ne l’est pas. Pour éviter des erreurs, insérez des nœuds à l’aide de la méthode [GetDescendant](../../t-sql/data-types/getdescendant-database-engine.md).
  
## <a name="data-type-conversion"></a>Conversion de types de données
Le type de données **hierarchyid** peut être converti en d’autres types de données comme suit :
-   Utilisez la méthode [ToString()](../../t-sql/data-types/tostring-database-engine.md) pour convertir la valeur **hierarchyid** en représentation logique comme type de données **nvarchar(4000)**.  
-   Utilisez [Read ()](../../t-sql/data-types/read-database-engine.md) et [Write ()](../../t-sql/data-types/write-database-engine.md) pour convertir **hierarchyid** en **varbinary**.  
-   Pour transmettre des paramètres **hierarchyid** par SOAP, convertissez-les d’abord en chaînes.  
  
## <a name="upgrading-databases"></a>Mise à niveau des bases de données
Quand une base de données est mise à niveau vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], le nouvel assembly et le type de données **hierarchyid** sont installés automatiquement. Les règles du Conseiller de mise à niveau détectent tout type d’utilisateur ou assembly avec des noms en conflit. Le Conseiller de mise à niveau recommandera de renommer tout assembly incompatible, et de renommer tout type en conflit ou d'utiliser des noms en deux parties dans le code pour faire référence à ce type d'utilisateur préexistant.
  
Si une mise à niveau de base de données détecte un assembly utilisateur avec un nom en conflit, elle renommera automatiquement cet assembly et placera la base de données en mode suspect.
  
Si un type d'utilisateur avec un nom en conflit existe pendant la mise à niveau, aucune étape spéciale n'est suivie. Après la mise à niveau, l'ancien type d'utilisateur et le nouveau type de système existeront tous deux. Le type d'utilisateur sera disponible uniquement en utilisant des noms en deux parties.
  
## <a name="using-hierarchyid-columns-in-replicated-tables"></a>Utilisation de colonnes hierarchyid dans les tables répliquées
Les colonnes de type **hierarchyid** peuvent être utilisées sur une table répliquée. La configuration requise pour votre application varie si la réplication est unidirectionnelle ou bidirectionnelle, et selon les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont utilisées.
  
### <a name="one-directional-replication"></a>Réplication unidirectionnelle
La réplication unidirectionnelle inclut les réplications d'instantanés, transactionnelle et de fusion dans laquelle les modifications ne sont pas apportées au niveau de l'abonné. Le fonctionnement des colonnes **hierarchyid** avec la réplication unidirectionnelle dépend de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée par l’abonné.
-   Un serveur de publication [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] peut répliquer des colonnes **hierarchyid** vers un abonné [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] sans considération spéciale.  
-   Un serveur de publication [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] doit convertir les colonnes **hierarchyid** pour les répliquer vers un abonné qui exécute [!INCLUDE[ssEW](../../includes/ssew-md.md)] ou une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] et les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prennent pas en charge les colonnes **hierarchyid**. Si vous utilisez l'une de ces versions, vous pouvez encore répliquer des données vers un abonné. Pour ce faire, vous devez définir une option de schéma ou le niveau de compatibilité de la publication (pour la réplication de fusion) afin que la colonne puisse être convertie en un type de données compatible.  
  
Le filtrage de colonne est pris en charge dans ces deux scénarios. Cela inclut l’exclusion des colonnes **hierarchyid**. Le filtrage de ligne est pris en charge tant que le filtre n’inclut pas de colonne **hierarchyid**.
  
### <a name="bi-directional-replication"></a>Réplication bidirectionnelle
La réplication bidirectionnelle inclut la réplication transactionnelle avec mise à jour des abonnements, la réplication transactionnelle d'égal à égal et la réplication de fusion dans laquelle les modifications sont apportées au niveau de l'abonné. La réplication vous permet de configurer une table avec des colonnes **hierarchyid** pour la réplication bidirectionnelle. Notez la configuration requise et les considérations suivantes :
-   Le serveur de publication et tous les abonnés doivent exécuter [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
-   La réplication réplique les données comme octets et n'effectue aucune validation pour assurer l'intégrité de la hiérarchie.  
-   La hiérarchie des modifications qui ont été apportées à la source (abonné ou serveur de publication) n'est pas conservée lorsqu'elles sont répliquées vers la destination.  
-   Les valeurs de hachage pour les colonnes **hierarchyid** sont spécifiques à la base de données dans laquelle elles sont générées. Par conséquent, la même valeur pourrait être générée sur le serveur de publication et l'abonné, mais pourrait être appliquée sur des lignes différentes. La réplication ne recherche pas cette condition et il n’existe aucune méthode intégrée pour partitionner les valeurs de la colonne **hierarchyid** comme c’est le cas pour les colonnes IDENTITY. Les applications doivent utiliser des contraintes ou d'autres mécanismes pour éviter de tels conflits non détectés.  
-   Il est possible que les lignes qui sont insérées sur l'abonné soient orphelines. La ligne parente de la ligne insérée a pu être supprimée au niveau du serveur de publication. Cela provoque un conflit non détecté lorsque la ligne de l'abonné est insérée au niveau du serveur de publication.  
-   Les filtres de colonne ne peuvent pas exclure les colonnes **hierarchyid** qui n’acceptent pas la valeur Null : les insertions de l’abonné échoueront, car il n’existe aucune valeur par défaut pour la colonne **hierarchyid** sur le serveur de publication.  
-   Le filtrage de ligne est pris en charge tant que le filtre n’inclut pas de colonne **hierarchyid**.  
  
## <a name="see-also"></a>Voir aussi
[Données hiérarchiques &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[Référence de méthodes de type de données hierarchyid]()
  
