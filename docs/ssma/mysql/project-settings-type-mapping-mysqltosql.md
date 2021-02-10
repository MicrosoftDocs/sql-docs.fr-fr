---
description: Paramètres du projet (Mappage de type) (MySQLToSQL)
title: Paramètres du projet (mappage de type) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 136fdf6d-657f-447b-af41-49bbc6e0e93e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a2f3e077a93d90afccf05023e41f8e5f2ea6fabc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017319"
---
# <a name="project-settings-type-mapping-mysqltosql"></a>Paramètres du projet (Mappage de type) (MySQLToSQL)
Les paramètres de projet de mappage de type vous permettent de définir des mappages de types par défaut pour le projet SSMA.  

Le mappage de type est disponible dans les boîtes de dialogue Paramètres du projet et paramètres du projet par défaut :  
  
-   Utilisez la boîte de dialogue Paramètres du projet pour définir les options de configuration du projet actif. Pour accéder aux paramètres de mappage de type, dans le menu Outils, sélectionnez Paramètres du projet, puis cliquez sur mappage de type dans le volet gauche.  
  
-   Utilisez la boîte de dialogue Paramètres du projet par défaut pour définir les options de configuration de tous les projets. Pour accéder aux paramètres de mappage de type, dans le menu Outils, sélectionnez Paramètres de projet par défaut, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés/Changed à partir de la liste déroulante de la **version cible** de la migration, puis cliquez sur mappage de type dans le volet gauche.  
  
## <a name="options"></a>Options  
  
##### <a name="source-type"></a>Type de source  
Il s’agit du type de données MySQL, qui doit être mappé au type de données de la base de données cible.  
  
##### <a name="target-type"></a>Type de cible  
Type de données de la base de données cible pour le type de données MySQL spécifié.  
  
##### <a name="add"></a>Ajouter  
Cliquez pour ajouter un type de données à la liste de mappage.  
  
##### <a name="edit"></a>Modifier  
Cliquez pour modifier le type de données sélectionné dans la liste mappage.  
  
##### <a name="remove"></a>Supprimer  
Cliquez pour supprimer le mappage de type de données sélectionné de la liste de mappage.  
  
##### <a name="reset-to-default"></a>Rétablir les valeurs par défaut  
Cliquez pour rétablir les valeurs par défaut SSMAs de la liste mappage de type.  
  
## <a name="type-mappings"></a>Mappages de types  
Le tableau suivant montre le mappage par défaut entre les types de données source et cible.  
  
|Type de données MySQL|Type de données de SQL Server|  
|-|-|  
|bigint|bigint|  
|BIGINT [*.. 255]|bigint|  
|binary|binaire [1]|  
|binaire [0.. 1]|binaire [1]|  
|binaire [2.. 255]|binaire [*]|  
|bit|binaire [1]|  
|bit [0.. 8]|binaire [1]|  
|bit [17.. 24]|binaire [3]|  
|bit [25.. 32]|binaire [4]|  
|bit [33.. 40]|binaire [5]|  
|bit [41.. 48]|binaire [6]|  
|bit [49.. 56]|binaire [7]|  
|bit [57.. 64]|binaire [8]|  
|bit [9.. 16]|binaire [2]|  
|objet BLOB|varbinary(max)|  
|objet blob [0.. 1]|varbinary [1]|  
|objet blob [2.. 8000]|varbinary [*]|  
|objet blob [8001.. *]|varbinary(max)|  
|bool|bit|  
|boolean|bit|  
|char|NCHAR [1]|  
|octet char|binaire [1]|  
|char Byte [0.. 1]|binaire [1]|  
|octet Char [2.. 255]|binaire [*]|  
|Char [0.. 1]|NCHAR [1]|  
|Char [2.. 255]|NCHAR [*]|  
|caractère|NCHAR [1]|  
|caractère variable [0.. 1]|nvarchar [1]|  
|caractère variable [2.. 255]|NVARCHAR|  
|caractère [0.. 1]|NCHAR [1]|  
|caractère [2.. 255]|NCHAR [*]|  
|Date|Date|  
|DATETIME|datetime2 [0]|  
|dec|Décimal|  
|Dec [*.. 65]|Decimal [*] [0]|  
|Dec [*.. 65] [ \* .. 30|Decimal [*] [ \* ]|  
|Décimal|Décimal|  
|Decimal [*.. 65]|Decimal [*] [0]|  
|Decimal [*.. 65] [ \* .. 30|Decimal [*] [ \* ]|  
|double|float [53]|  
|double précision|float [53]|  
|double précision [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|double [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|fixe|numeric|  
|correction de [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|float|float [24]|  
|float [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|float [*.. 53]|float [53]|  
|int|int|  
|int [*.. 255]|int|  
|entier|int|  
|entier [*.. 255]|int|  
|longblob|varbinary(max)|  
|longtext|nvarchar(max)|  
|mediumblob|varbinary(max)|  
|mediumint|int|  
|mediumint[*.. 255]|int|  
|mediumtext|nvarchar(max)|  
|caractère national|NCHAR [1]|  
|caractère national [0.. 1]|NCHAR [1]|  
|caractère national [2.. 255]|NCHAR [*]|  
|caractère national|NCHAR [1]|  
|caractère national variable|nvarchar [1]|  
|caractère national variable [0.. 1]|nvarchar [1]|  
|caractère national variable [2.. 4000]|nvarchar [*]|  
|caractère national variable [4001.. *]|nvarchar(max)|  
|caractère national [0.. 1]|NCHAR [1]|  
|caractère national [2.. 255]|NCHAR [*]|  
|varchar national|nvarchar [1]|  
|national VARCHAR [0.. 1]|nvarchar [1]|  
|national VARCHAR [2.. 4000]|nvarchar [*]|  
|national VARCHAR [4001.. *]|nvarchar(max)|  
|NCHAR|NCHAR [1]|  
|NCHAR varchar|nvarchar [1]|  
|NCHAR VARCHAR [0.. 1]|nvarchar [1]|  
|NCHAR VARCHAR [2.. 4000]|nvarchar [*]|  
|NCHAR VARCHAR [4001.. *]|nvarchar(max)|  
|NCHAR [0.. 1]|NCHAR [1]|  
|NCHAR [2.. 255]|NCHAR [*]|  
|numeric|numeric|  
|Numeric [*.. 65]|Numeric [*] [0]|  
|Numeric [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|NVARCHAR|nvarchar [1]|  
|nvarchar [0.. 1]|nvarchar [1]|  
|nvarchar [2.. 4000]|nvarchar [*]|  
|nvarchar [4001.. *]|nvarchar(max)|  
|real|float [53]|  
|réel [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|serial|bigint|  
|SMALLINT|SMALLINT|  
|smallint [*.. 255]|SMALLINT|  
|text|nvarchar(max)|  
|texte [0.. 1]|nvarchar [1]|  
|texte [2.. 4000]|nvarchar [*]|  
|texte [4001.. *]|nvarchar(max)|  
|time|time|  
|timestamp|DATETIME|  
|tinyblob|varbinary [255]|  
|TINYINT|SMALLINT|  
|tinyint [*.. 255]|SMALLINT|  
|tinytext|nvarchar [255]|  
|unsigned bigint|bigint|  
|unsigned BIGINT [*.. 255]|bigint|  
|non signé Dec|Décimal|  
|non signé Dec [*.. 65]|Decimal [*] [0]|  
|non signé Dec [*.. 65] [ \* .. 30|Decimal [*] [ \* ]|  
|décimal non signé|Décimal|  
|décimal non signé [*.. 65]|Decimal [*] [0]|  
|décimal non signé [*.. 65] [ \* .. 30|Decimal [*] [ \* ]|  
|double non signé|float [53]|  
|double précision non signée|float [53]|  
|double précision non signée [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|double non signé [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|non signé fixe|numeric|  
|fixed non signé [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|float non signé|float [24]|  
|unsigned float [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|unsigned float [*.. 53]|float [53]|  
|nombre entier non signé|bigint|  
|unsigned int [*.. 255]|bigint|  
|entier non signé|bigint|  
|entier non signé [*.. 255]|bigint|  
|unsigned MEDIUMINT|int|  
|unsigned MEDIUMINT [*.. 255]|int|  
|valeur numérique non signée|numeric|  
|Numeric non signé [*.. 65]|Numeric [*] [0]|  
|Numeric non signé [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|réel non signé|float [53]|  
|réel non signé [*.. 255 [[ \* .. 30|Numeric [*] [ \* ]|  
|unsigned smallint|int|  
|unsigned smallint [*.. 255]|int|  
|unsigned tinyint|TINYINT|  
|unsigned tinyint [*.. 255]|TINYINT|  
|varbinary [0.. 1]|varbinary [1]|  
|varbinary [2.. 8000]|varbinary [*]|  
|varbinary [8001.. *]|varbinary(max)|  
|VARCHAR [0.. 1]|nvarchar [1]|  
|VARCHAR [2.. 4000]|nvarchar [*]|  
|VARCHAR [4001.. *]|nvarchar(max)|  
|year|SMALLINT|  
|année [2.. 2]|SMALLINT|  
|année [4.. 4]|SMALLINT|  
  
##### <a name="add"></a>Ajouter  
Cliquez pour ajouter un type de données à la liste de mappage.  
  
##### <a name="edit"></a>Modifier  
Cliquez pour modifier un type de données dans la liste mappage.  
  
##### <a name="remove"></a>Supprimer  
Cliquez pour supprimer le mappage de type de données sélectionné de la liste de mappage.  
  
##### <a name="reset-to-default"></a>Rétablir les valeurs par défaut  
Cliquez pour réinitialiser tous les mappages de type de données aux paramètres SSMA par défaut.  
  
