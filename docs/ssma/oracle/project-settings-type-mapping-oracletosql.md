---
description: Paramètres du projet (Mappage de type) (OracleToSQL)
title: Paramètres du projet (mappage de type) (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4bb8466e-2199-4f00-8513-b04e9586723d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: cbb47d74535af0dea97842bdf46680a4376c2ba7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067730"
---
# <a name="project-settings-type-mapping-oracletosql"></a>Paramètres du projet (Mappage de type) (OracleToSQL)
La page mappage de type de la boîte de dialogue **paramètres du projet** contient des paramètres qui personnalisent la manière dont SSMA convertit les types de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de données.  
  
La page mappage de type est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Pour spécifier les paramètres de tous les futurs projets SSMA, dans le menu **Outils** , cliquez sur **paramètres du projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés ou modifiés dans la liste déroulante de la **version cible** de la migration, puis cliquez sur **mappage de type** en bas du volet gauche.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , cliquez sur **paramètres du projet**, puis sur mappage de **type** en bas du volet gauche.  
  
Pour spécifier les paramètres de l’objet ou de la classe d’objets en cours, utilisez l’onglet **mappage de type** dans la fenêtre SSMA principale.  
  
## <a name="options"></a>Options  
Le tableau suivant présente les options de l’onglet **mappage de type** :  
  
**Type de source**  
Type de données Oracle mappé.  
  
**Type de cible**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Type de données cible pour le type de données Oracle spécifié.  
  
Consultez les tableaux de la section suivante pour les mappages de type SSMA pour Oracle par défaut.  
  
**Ajouter**  
Cliquez pour ajouter un type de données à la liste de mappage.  
  
**Modifier**  
Cliquez pour modifier le type de données sélectionné dans la liste mappage.  
  
**Supprimer**  
Cliquez pour supprimer le mappage de type de données sélectionné de la liste de mappage.  
  
**Rétablir les valeurs par défaut**  
Cliquez pour rétablir les valeurs par défaut SSMAs de la liste mappage de type.  
  
## <a name="default-type-mappings"></a>Mappages de types par défaut  
Dans SSMA pour Oracle, vous pouvez définir des mappages de types personnalisés pour les arguments, les colonnes, les variables locales et les valeurs de retour. Le mappage par défaut pour les arguments et les types de retour est presque identique.  
  
### <a name="default-argument-type-and-return-value-type-mapping"></a>Type d’argument par défaut et mappage de type de valeur de retour  
Le tableau suivant contient le mappage de type de données par défaut pour les arguments et les valeurs de retour.  
  
|Type de données Oracle|Type de données par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|--------------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_integer|int|  
|objet BLOB|varbinary(max)|  
|boolean|bit|  
|char|varchar(max)|  
|char varying|varchar(max)|  
|caractère|varchar(max)|  
|character varying|varchar(max)|  
|CLOB|varchar(max)|  
|Date|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Décimal|float [53]|  
|double précision|float [53]|  
|float|float [53]|  
|int|int|  
|entier|int|  
|long|varchar(max)|  
|long RAW|varbinary(max)|  
|long RAW [ \* .. 8000]<sup>*</sup>|varbinary [*]|  
|long RAW [8001.. \* ]<sup>*</sup>|varbinary(max)|  
|caractère national|nvarchar(max)|  
|caractère national variable|nvarchar(max)|  
|caractère national|nvarchar(max)|  
|caractère national variable<sup>**</sup>|nvarchar(max)|  
|caractère national variable<sup>*</sup>|nvarchar(max)|  
|NCHAR|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|nombre|float [53]|  
|numeric|float [53]|  
|nvarchar2|nvarchar(max)|  
|pls_integer|int|  
|raw|varbinary(max)|  
|real|float [53]|  
|ID|UNIQUEIDENTIFIER|  
|signtype|SMALLINT|  
|SMALLINT|SMALLINT|  
|string|varchar(max)|  
|timestamp|datetime2|  
|horodateur avec fuseau horaire local|datetimeoffset|  
|horodateur avec fuseau horaire|datetimeoffset|  
|UROWID|UNIQUEIDENTIFIER|  
|varchar|varchar(max)|  
|VARCHAR2|varchar(max)|  
|XmlType|Xml|  
  
<sup>*</sup> S’applique uniquement au mappage de type de valeur de retour.  
  
<sup>**</sup> S’applique uniquement au mappage de type d’argument.  
  
### <a name="default-column-type-mapping"></a>Mappage de type de colonne par défaut  
Le tableau suivant contient le mappage de type par défaut pour les colonnes.  
  
|Type de données Oracle|Type de données par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|--------------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|objet BLOB|varbinary(max)|  
|char|char|  
|char varying [*.. \* ]|VARCHAR [*]|  
|Char [*.. \* ]|Char [*]|  
|caractère|char|  
|caractère variable [*.. \* ]|VARCHAR [*]|  
|caractère [*.. \* ]|Char [*]|  
|CLOB|varchar(max)|  
|Date|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Dec [*.. \* ]|Dec [*] [0]|  
|Dec [*.. \* ] [\*..\*]|Dec [*] [ \* ]|  
|Décimal|décimal [38] [0]|  
|Decimal [*.. \* ]|Decimal [*] [0]|  
|Decimal [*.. \* ] [\*..\*]|Decimal [*] [ \* ]|  
|double précision|float [53]|  
|float|float [53]|  
|float [*.. 53]|float [*]|  
|float [54.. *]|float [53]|  
|int|int|  
|entier|int|  
|long|varchar(max)|  
|long RAW|varbinary(max)|  
|long RAW [*.. 8000]|varbinary [*]|  
|long RAW [8001.. *]|varbinary(max)|  
|long varchar|varchar(max)|  
|long [*.. 8000]|VARCHAR [*]|  
|long [8001.. *]|varchar(max)|  
|caractère national|NCHAR|  
|caractère national variable [*.. \* ]|nvarchar [*]|  
|caractère national [*.. \* ]|NCHAR [*]|  
|caractère national|NCHAR|  
|caractère national variable [*.. \* ]|nvarchar [*]|  
|caractère national [*.. \* ]|NCHAR [*]|  
|NCHAR|NCHAR|  
|NCHAR [*]|NCHAR [*]|  
|NCLOB|nvarchar(max)|  
|nombre|float [53]|  
|nombre [*.. \* ]|Numeric [*]|  
|nombre [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|numeric|numeric|  
|numérique [*.. \* ]|Numeric [*]|  
|numérique [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|NVARCHAR2 [*.. \* ]|nvarchar [*]|  
|RAW [*.. \* ]|varbinary [*]|  
|real|float [53]|  
|ID|UNIQUEIDENTIFIER|  
|SMALLINT|SMALLINT|  
|timestamp|datetime2|  
|horodateur avec fuseau horaire local|datetimeoffset|  
|horodateur avec fuseau horaire local [*.. \* ]|DateTimeOffset [*]|  
|horodateur avec fuseau horaire|datetimeoffset|  
|horodateur avec fuseau horaire [*.. \* ]|DateTimeOffset [*]|  
|horodateur [*.. \* ]|datetime2 [*]|  
|UROWID|UNIQUEIDENTIFIER|  
|UROWID [*.. \* ]|UNIQUEIDENTIFIER|  
|VARCHAR [*.. \* ]|VARCHAR [*]|  
|VARCHAR2 [*.. \* ]|VARCHAR [*]|  
|XmlType|Xml|  
  
### <a name="default-local-variable-type-mapping"></a>Mappage de type de variable locale par défaut  
Le tableau suivant contient le mappage de type par défaut pour les variables locales.  
  
|Type de données Oracle|Type de données par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|--------------------|-------------------------------------------------------------------------|  
|Bfile|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_interger|int|  
|Objet blob|varbinary(max)|  
|Booléen|bit|  
|Char|char|  
|char varying [*.. 8000]|VARCHAR [*]|  
|char varying [8001.. *]|varchar(max)|  
|Char [*.. 8000]|Char [*]|  
|Char [8001.. *]|varchar(max)|  
|Caractère|char|  
|caractère variable [*.. 8000]|VARCHAR [*]|  
|caractère variable [8001.. *]|varchar(max)|  
|caractère [*.. 8000]|Char [*]|  
|caractère [8001.. *]|varchar(max)|  
|CLOB|varchar(max)|  
|Date|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Dec [*.. \* ]|Dec [*] [0]|  
|Dec [*.. \* ] [\*..\*]|Dec [*] [ \* ]|  
|Décimal|décimal [38] [0]|  
|Decimal [*.. \* ]|Decimal [*] [0]|  
|Decimal [*.. \* ] [\*..\*]|Decimal [*] [ \* ]|  
|double précision|float [53]|  
|Float|float [53]|  
|float [*.. 53]|float [*]|  
|float [54.. *]|float [53]|  
|Int|int|  
|Integer|int|  
|entier [*.. \* ]|Numeric [*] [0]|  
|Long|varchar(max)|  
|long RAW|varbinary(max)|  
|long RAW [*.. 8000]|varbinary [*]|  
|long RAW [8001.. *]|varbinary(max)|  
|caractère national|NCHAR|  
|caractère national variable [*.. 4000]|nvarchar [*]|  
|caractère national variable [4001.. *]|nvarchar(max)|  
|caractère national [*.. 4000]|NCHAR [*]|  
|caractère national [4001.. *]|nvarchar(max)|  
|caractère national|NCHAR|  
|caractère national [*.. 4000]|nvarchar [*]|  
|caractère national [4001.. *]|nvarchar(max)|  
|caractère national variable [*.. 4000]|nvarchar [*]|  
|caractère national variable [4001.. *]|nvarchar(max)|  
|Nchar|NCHAR|  
|NCHAR [*.. 4000]|NCHAR [*]|  
|NCHAR [4001.. *]|nvarchar(max)|  
|NCHAR varying [*.. 4000]|nvarchar [*]|  
|NCHAR varying [4001.. *]|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|Nombre|float [53]|  
|nombre [*.. \* ]|Numeric [*]|  
|nombre [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|Numérique|numérique [38] [0]|  
|numérique [*.. \* ]|Numeric [*]|  
|numérique [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|nvarchar2[*.. 4000]|nvarchar [*]|  
|NVARCHAR2 [4001.. *]|nvarchar(max)|  
|pls_integer|int|  
|RAW [*.. 8000]|varbinary [*]|  
|RAW [8001.. *]|varbinary(max)|  
|Real|float [53]|  
|Rowid|UNIQUEIDENTIFIER|  
|Signtype|SMALLINT|  
|Smallint|SMALLINT|  
|chaîne [*.. 8000]|VARCHAR [*]|  
|chaîne [8001.. *]|varchar(max)|  
|timestamp|datetime2|  
|horodateur avec fuseau horaire local|datetimeoffset|  
|horodateur avec fuseau horaire|datetimeoffset|  
|horodateur avec fuseau horaire local [*.. \* ]|DateTimeOffset [*]|  
|horodateur avec fuseau horaire [*.. \* ]|DateTimeOffset [*]|  
|horodateur [*.. \* ]|datetime2 [*]|  
|UROWID|UNIQUEIDENTIFIER|  
|UROWID [*.. \* ]|UNIQUEIDENTIFIER|  
|VARCHAR [*.. 8000]|VARCHAR [*]|  
|VARCHAR [8001.. *]|varchar(max)|  
|VARCHAR2 [*.. 8000]|VARCHAR [*]|  
|VARCHAR2 [8001.. *]|varcha (max)|  
|XmlType|Xml|  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’interface utilisateur &#40;OracleToSQL&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
