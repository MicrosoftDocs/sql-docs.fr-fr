---
description: Paramètres du projet (Conversion) (MySQLToSQL)
title: Paramètres du projet (conversion) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7ad5fe44-6445-4ba8-a457-5af792631f11
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7cdbc6ac65fa24f8130bc92c03b62145370bedea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017419"
---
# <a name="project-settings-conversion-mysqltosql"></a>Paramètres du projet (Conversion) (MySQLToSQL)
La page conversion de la boîte de dialogue **paramètres du projet** contient des paramètres qui personnalisent la manière dont SSMA convertit la syntaxe MySQL en SQL Server ou SQL Azure syntaxe.  
  
Le volet conversion est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Utilisez la boîte de dialogue **paramètres du projet par défaut** pour définir les options de configuration de tous les projets. Pour accéder aux paramètres de conversion, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés/Changed à partir de la liste déroulante de la **version cible** de la migration, cliquez sur **général** en bas du volet gauche, puis sélectionnez **conversion**.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , cliquez sur **paramètres du projet**, puis sur **général** en bas du volet gauche, et enfin sur **conversion**.  
  
## <a name="options"></a>Options  
  
### <a name="collate-clause"></a>COLLATE, clause  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Conversion de clause COLLATE explicite**|L’option de conversion de clause COLLATE explicite spécifie comment convertir des clauses de classement explicites dans du code MySQL. Choix possibles : ignorer et marquer avec un avertissement/générer une erreur<br /><br />**Mode par défaut**: ignorer et marquer avec un avertissement<br /><br />**Mode optimiste**: ignorer et marquer avec un avertissement<br /><br />**Mode complet**: ignorer et marquer avec un avertissement|  
  
### <a name="column-constraints"></a>Contraintes de colonne  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Générer la contrainte pour les colonnes de type de données ENUM**|Génère une contrainte pour les colonnes de type de données ENUM dans le SQL Server ou SQL Azure table, si elle n’est pas présente dans la table MySQL. Si c’est le cas, toutes les colonnes converties du type de données ENUM sont accompagnées de la contrainte CHECK contrôlant la valeur.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: Oui|  
|**Générer la contrainte pour les colonnes de type de données SET**|Génère une contrainte pour les colonnes de type de données SET dans le SQL Server ou SQL Azure table, si elle n’est pas présente dans la table MySQL. Si c’est le cas, toutes les colonnes converties du type de données SET sont accompagnées de la contrainte CHECK contrôlant la valeur.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: Oui|  
|**Générer une contrainte pour des colonnes de colonnes de type de données numériques non SIGNÉes**|Ajoutez un contrôle de valeur non négative aux colonnes des types de données numériques non SIGNÉs.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: Oui|  
|**Générer la contrainte pour les colonnes de type de données YEAR**|Génère une contrainte pour les colonnes de type de données YEAR dans la table SQL Server ou SQL Azure, si elle n’est pas présente dans la table MySQL. Si c’est le cas, toutes les colonnes converties du type de données YEAR sont accompagnées de la contrainte CHECK contrôlant la valeur.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: Oui|  
  
### <a name="data-types"></a>Types de données  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Conversion de type de données ENUM**|Spécifie le mode de conversion du type de données de l’ÉNUMÉRation MySQL en tant que Convert en NVARCHAR ou Convert en Numeric<br /><br />**Mode par défaut**: convertir en nvarchar<br /><br />**Mode optimiste**: convertir en nvarchar<br /><br />**Mode complet**: convertir en nvarchar|  
|**DÉFINIR la conversion des types de données**|Spécifie comment le type de données SET MySQL doit être converti, convertir en NVARCHAR (L)/Convert en BINARY (L)<br /><br />**Mode par défaut**: convertir en nvarchar (L)<br /><br />**Mode optimiste**: convertir en nvarchar (L)<br /><br />**Mode complet**: convertir en nvarchar (L)|  
  
### <a name="generic"></a>Générique  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Colonnes sans valeur par défaut dans INSERT et replace**|Si la valeur est « Yes », toutes les instructions qui référencent des tables à l’aide de moteurs stockés autres que MyISAM et InnoDb doivent être marquées avec des messages de conversion d’avertissement.<br /><br />**Mode par défaut**: ajouter à la liste des colonnes<br /><br />**Mode optimiste**: ajouter à la liste des colonnes<br /><br />**Mode complet**: ajouter à la liste des colonnes|  
|**La division par zéro produit une conversion**|Spécifie s’il faut ou non émuler MySQL sans ERROR_FOR_DIVISION_BY_ZERO comportement.<br /><br />**Mode par défaut**: erreur<br /><br />**Mode optimiste**: erreur<br /><br />**Mode complet**: null|  
|**Opérateur IN**|Spécifie comment convertir MySQL dans l’opérateur.<br /><br />**Mode par défaut**: toujours convertir en in<br /><br />**Mode optimiste**: toujours convertir en in<br /><br />**Mode complet**: développer si nécessaire|  
|**Conversion de fonction MySQL**|Spécifie comment convertir des fonctions MySQL standard.<br /><br />**Mode par défaut**: optimiste<br /><br />**Mode optimiste**: optimiste<br /><br />**Mode complet**: précis|  
|**Moteurs de stockage non pris en charge**|Si la valeur est « Yes », toutes les instructions qui référencent des tables à l’aide de moteurs stockés autres que MyISAM et InnoDb doivent être marquées avec des messages de conversion d’avertissement.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: Oui|  
|**Supprimer la génération de colonne auxiliaire ROWID**|Si oui, interdit la création de la création de colonne auxiliaire ROWD sur les tables cibles. Peut affecter la migration de certaines structures.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: non|  
|**Conversion d’instruction TRUNCATE**|Spécifie comment convertir des instructions Truncate.<br /><br />**Mode par défaut**: tronquer<br /><br />**Mode optimiste**: tronquer<br /><br />**Mode complet**: tronquer|  
  
### <a name="misc"></a>Divers  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Mappage de schéma par défaut**|Spécifie comment mapper des bases de données MySQL dans des schémas de SQL Server.<br /><br />**Mode par défaut**: base de données à base de données<br /><br />**Mode optimiste**: base de données vers base de données<br /><br />**Mode complet**: base de données dans la base de données|  
  
### <a name="procedures-and-functions"></a>Procédures et fonctions  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Conversion de fonction par défaut**|Spécifie si les fonctions doivent être converties par défaut en fonctions T-SQL ou en procédures stockées.<br /><br />**Mode par défaut**: convertir en fonction<br /><br />**Mode optimiste**: convertir en fonction<br /><br />**Mode complet**: convertir en fonction|  
|**Générer une XACT_ABORT de jeu activée**|Spécifie si XACT_ABORT doit être ajouté au début de la procédure ou du déclencheur converti.<br /><br />**Mode par défaut**: Oui<br /><br />**Mode optimiste**: Oui<br /><br />**Mode complet**: Oui|  
|**Générer SET NOCOUNT ON**|Spécifie si SET NOCOUNT ON doit être ajouté au début de la procédure ou du déclencheur converti.<br /><br />**Mode par défaut**: Oui<br /><br />**Mode optimiste**: Oui<br /><br />**Mode complet**: Oui|  
  
### <a name="spatial-data-types"></a>Types de données spatiales  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Cadre englobant par défaut {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} pour les index spatiaux**|Définit la valeur par défaut pour le paramètre {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} du cadre englobant utilisé dans les index spatiaux.<br /><br />**Mode par défaut**<br /><br />XMAX : 100<br /><br />XMIN : 0<br /><br />YMAX : 100<br /><br />YMIN : 0<br /><br />**Mode optimiste**<br /><br />XMAX : 100<br /><br />XMIN : 0<br /><br />YMAX : 100<br /><br />YMIN : 0<br /><br />**Mode complet**<br /><br />XMAX : 100<br /><br />XMIN : 0<br /><br />YMAX : 100<br /><br />YMIN : 0|  
|**Densité de grille par défaut pour les index spatiaux**|Définit la valeur par défaut pour LEVEL_1, LEVEL_2, LEVEL_3 et LEVEL_4 de la densité de la grille utilisée dans les index spatiaux.<br /><br />**Mode par défaut**<br /><br />LEVEL_1 : par défaut<br /><br />LEVEL_2 : par défaut<br /><br />LEVEL_3 : par défaut<br /><br />LEVEL_4 : par défaut<br /><br />**Mode optimiste**<br /><br />LEVEL_1 : par défaut<br /><br />LEVEL_2 : par défaut<br /><br />LEVEL_3 : par défaut<br /><br />LEVEL_4 : par défaut<br /><br />**Mode complet**<br /><br />LEVEL_1 : par défaut<br /><br />LEVEL_2 : par défaut<br /><br />LEVEL_3 : par défaut<br /><br />LEVEL_4 : par défaut|  
  
### <a name="transactions"></a>Transactions  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Tables non transactionnelles**|Spécifie si toutes les références à une table qui ne prennent pas en charge les transactions doivent être marquées avec des messages de conversion d’avertissement.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: Oui|  
|**Niveau d’isolation de la transaction**|Spécifie le niveau d’isolation de la transaction à utiliser pour les nouvelles transactions.<br /><br />**Mode par défaut**: par défaut<br /><br />**Mode optimiste**: par défaut<br /><br />**Mode complet**: lecture renouvelable|  
  
### <a name="value-control"></a>Contrôle de valeur  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Conversion de caractère à numérique**|Spécifie comment gérer les conversions implicites et explicites du type de données character en types de données numériques.<br /><br />**Mode par défaut**: optimiste<br /><br />**Mode optimiste**: optimiste<br /><br />**Mode complet**: précis|  
|**Contrôler les valeurs numériques non SIGNÉes**|Contrôle assignant des valeurs à des variables numériques non SIGNÉes et à des paramètres.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: Oui|  
|**Contrôler la soustraction non signée**|Modifiez les valeurs négatives insérées dans les colonnes de table de type de données non signé.<br /><br />**Mode par défaut**: convertir « en l’or »<br /><br />**Mode optimiste**: convertir « en l’or »<br /><br />**Mode complet**: signal avec un avertissement|  
|**Conversion vers et à partir d’un type de données binary**|Spécifie comment gérer la conversion implicite et explicite à partir d’un type de données binary.<br /><br />**Mode par défaut**: optimiste<br /><br />**Mode optimiste**: optimiste<br /><br />**Mode complet**: précis|  
|**Conversion en type de données date/heure**|Spécifie comment gérer la conversion implicite et explicite vers le type de données date/heure.<br /><br />**Mode par défaut**: émuler le format MySQL<br /><br />**Mode optimiste**: utilisez le format SQL Server<br /><br />**Mode complet**: émuler le format MySQL|  
|**Littéraux numériques avec une précision supérieure à 38**|Spécifie comment convertir des littéraux numériques avec une précision supérieure à 38.<br /><br />**Mode par défaut**: arrondir si possible<br /><br />**Mode optimiste**: arrondir si possible<br /><br />**Mode complet**: arrondir si possible|  
|**Zero-date dans les colonnes NOT NULL**|Spécifie comment gérer l’assignation à des colonnes non NULL de valeurs de date/heure zéro ou de date zéro.<br /><br />**Mode par défaut**: GETDATE ()<br /><br />**Mode optimiste**: GETDATE ()<br /><br />**Mode complet**: GETDATE ()|  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’interface utilisateur &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
  
