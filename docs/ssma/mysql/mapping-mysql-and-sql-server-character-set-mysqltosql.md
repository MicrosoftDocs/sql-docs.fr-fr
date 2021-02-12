---
title: Mappage de MySQL et SQL Server jeu de caractères (MySQLToSQL) | Microsoft Docs
description: Découvrez comment spécifier un jeu de caractères pour les types de données de caractères MySQL, les expressions et les littéraux à utiliser pendant la conversion de type de données caractère par SSMA pour MySQL.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 20b3f22e-16a2-4a87-b4eb-c277be6bf5c8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 13b2accd6b4dc2ba2bbd8c9c1e6db312650d2d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077620"
---
# <a name="mapping-mysql-and-sql-server-character-set-mysqltosql"></a>Mappage des jeux de caractères MySQL et SQL Server (MySQLToSQL)
Le jeu de caractères (CharSet) peut être spécifié pour les types de données de caractères MySQL, les expressions et les littéraux.  
  
## <a name="charset-mapping"></a>Mappage de jeux de caractères  
Le mappage charset est défini pour chaque jeu de caractères MySQL et utilisé lors de la conversion du type de données character. Elle indique comment convertir les types de données de chaîne de caractères d’un jeu de caractères particulier :  
  
-   En types de caractères de SQL Server nationaux (NCHAR/NVARCHAR) ou  
  
-   Pour SQL Server des types de caractères standard (CHAR/VARCHAR)  
  
1.  les types de données de caractères de la base de données cible **nationale** sont :  
  
    1.  **nchar**  
  
    2.  **nvarchar**  
  
2.  les types de données de caractères de base de données cible **standard** sont :  
  
    1.  **char**  
  
    2.  **varchar**  
  
3.  Le mappage de type autorise uniquement le mappage aux types de données caractère **nationaux** . Après que le type de données caractère MySQL est converti selon le mappage de type, le mappage charset est appliqué.  
  
> [!NOTE]  
> Le mappage de jeu de caractères peut être défini sur chaque niveau de nœud de l’Explorateur d’objets de métadonnées et représenter tous les jeux de caractères lus à partir de MySQL.  
  
## <a name="charset-mapping-on-different-node-levels"></a>Mappage de jeux de caractères sur différents niveaux de nœud  
Le mappage de jeu de caractères varie selon les différents niveaux de nœud, à savoir :  
  
1.  Au niveau du nœud de métadonnées racine  
  
2.  Au niveau de la base de données, de la catégorie et des nœuds d’objet  
  
> [!NOTE]  
> L’onglet sélectionné pour modifier le mappage de jeu de caractères contient trois boutons, quel que soit le mappage sur les différents niveaux de nœud.  
>   
> Il s'agit de :  
>   
> 1.  **Appliquer :** Applique les modifications apportées par l’utilisateur, activé uniquement quand le mappage charset est modifié et n’est pas encore enregistré.  
> 2.  **Annuler :** Annule les modifications apportées par l’utilisateur. Le bouton est activé lorsque le mappage charset est modifié mais pas enregistré.  
> 3.  **Rétablir les valeurs par défaut :** Rétablit les valeurs par défaut de tous les mappages.  
  
1.  **Au niveau du nœud de métadonnées racine :**  La grille de mappage charset contient une grille de jeux de caractères avec une colonne distincte pour chaque jeu de caractères. Les colonnes de la grille sont les suivantes :  
  
    1.  La première colonne de la grille nommée **charset** contient le nom du jeu de caractères.  
  
    2.  La seconde **Description charset** contient une description CharSet.  
  
    3.  La troisième colonne intitulée, **type de jeu de caractères cible** contient des paramètres de mappage pour un jeu de caractères particulier. Les valeurs de cette colonne sont les suivantes :  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
    > [!IMPORTANT]  
    > Les valeurs par défaut d’un jeu de caractères particulier ont le préfixe « (Default) » après CHAR/VARCHAR ou NCHAR/NVARCHAR.  
  
    Le mappage de jeu de caractères entre la base de données MySQL et la base de données cible sur le nœud de métadonnées racine est indiqué ci-dessous :  
  
    |Nom du jeu de caractères|Description du jeu de caractères|Type de jeu de caractères cible (par défaut)|  
    |-|-|-|  
    |Big5|Chinois traditionnel BIG5|NCHAR/NVARCHAR (par défaut)|  
    |dec8|DEC West Europe|CHAR/VARCHAR (valeur par défaut)|  
    |cp850|DOS ouest des États-européens|CHAR/VARCHAR (valeur par défaut)|  
    |hp8|HP West Europe|CHAR/VARCHAR (valeur par défaut)|  
    |koi8r|KOI8-R Relcom russe|CHAR/VARCHAR (valeur par défaut)|  
    |Latin 1|cp1252-Europe de l’ouest|CHAR/VARCHAR (valeur par défaut)|  
    |latin2|ISO 8859-2-Europe centrale|CHAR/VARCHAR (valeur par défaut)|  
    |swe7|7bit suédois|CHAR/VARCHAR (valeur par défaut)|  
    |ascii|US ASCII|CHAR/VARCHAR (valeur par défaut)|  
    |ujis|EUC-JP japonais|NCHAR/NVARCHAR (par défaut)|  
    |sjis|Shift-JIS japonais|NCHAR/NVARCHAR (par défaut)|  
    |Hébreu|ISO 8859-8 hébreu|CHAR/VARCHAR (valeur par défaut)|  
    |tis620|TIS620 thaï|CHAR/VARCHAR (valeur par défaut)|  
    |euckr|EUC-KR coréen|NCHAR/NVARCHAR (par défaut)|  
    |koi8u|KOI8-U-ukrainien|CHAR/VARCHAR (valeur par défaut)|  
    |GB2312|Chinois simplifié GB2312|NCHAR/NVARCHAR (par défaut)|  
    |grec|ISO 8859-7 grec|CHAR/VARCHAR (valeur par défaut)|  
    |CP 1250|Europe centrale de Windows|CHAR/VARCHAR (valeur par défaut)|  
    |GBK|Chinois simplifié GBK|NCHAR/NVARCHAR (par défaut)|  
    |latin5|ISO 8859-9 turc|CHAR/VARCHAR (valeur par défaut)|  
    |armscii8|ARMSCII-8 arménien|CHAR/VARCHAR (valeur par défaut)|  
    |utf8|Unicode UTF-8|NCHAR/NVARCHAR (par défaut)|  
    |ucs2|Unicode UCS-2|NCHAR/NVARCHAR (par défaut)|  
    |cp866|DOS (russe)|CHAR/VARCHAR (valeur par défaut)|  
    |keybcs2|Czech-Slovak DOS Kamenicky|CHAR/VARCHAR (valeur par défaut)|  
    |macce|Europe centrale Mac|CHAR/VARCHAR (valeur par défaut)|  
    |macro|Europe de l’Ouest Mac|CHAR/VARCHAR (valeur par défaut)|  
    |cp852|DOS-Europe centrale|CHAR/VARCHAR (valeur par défaut)|  
    |latin7|ISO 8859-13 Baltique|CHAR/VARCHAR (valeur par défaut)|  
    |CP 1251|Windows cyrillique|CHAR/VARCHAR (valeur par défaut)|  
    |CP 1256|Arabe Windows|CHAR/VARCHAR (valeur par défaut)|  
    |CP 1257|Balte Windows|CHAR/VARCHAR (valeur par défaut)|  
    |binary|Pseudo-jeu binaire|CHAR/VARCHAR (valeur par défaut)|  
    |geostd8|Géorgien GEOSTD8|CHAR/VARCHAR (valeur par défaut)|  
    |cp932|SJIS pour Windows (japonais)|NCHAR/NVARCHAR (par défaut)|  
    |eucjpms|UJIS pour Windows (japonais)|NCHAR/NVARCHAR (par défaut)|  
  
2.  Au **niveau de la base de données, de la catégorie ou du nœud d’objet :** Dans le niveau de la base de données, de la catégorie ou des nœuds objet, la grille de mappage charset contient les mêmes lignes que le niveau de nœud de métadonnées racine, c.-à-d. :  
  
    1.  La première colonne de la grille intitulée, **nom du jeu de caractères** contient le nom du jeu de caractères.  
  
    2.  La deuxième colonne intitulée, **Description du jeu de caractères** contient la description du jeu de caractères.  
  
    3.  La seule différence est les valeurs de la troisième colonne de la grille. La troisième colonne intitulée type de **données cible** contient des paramètres de mappage pour un jeu de caractères particulier. Les valeurs de la colonne sont les suivantes :  
  
        -   Inherited (CHAR/VARCHAR ou NCHAR/NVARCHAR)  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
> [!IMPORTANT]  
> -   Dans le mappage de jeu de caractères entre la base de données MySQL et la base de données cible sur les niveaux de nœuds de base de données, de catégorie et d’objet, les valeurs par défaut d’un jeu de caractères particulier pour chaque niveau autre que la racine pour le **type de données de cible** de colonne doivent être « hérité ».  
> -   Dans la grille, la valeur **héritée** est suivie d’un suffixe' (char/varchar) 'ou' (NCHAR/NVARCHAR) 'en fonction de la valeur héritée du parent par ce jeu de caractères particulier.  
  
