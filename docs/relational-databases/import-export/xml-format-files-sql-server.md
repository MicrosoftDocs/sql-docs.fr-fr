---
title: Fichiers de format XML (SQL Server) | Microsoft Docs
description: SQL Server 2019 fournit un schéma XML qui définit la syntaxe des fichiers de format XML à utiliser pour l’importation en bloc de données dans une table SQL Server.
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], XML format files
- bulk importing [SQL Server], format files
- XML format files [SQL Server]
ms.assetid: 69024aad-eeea-4187-8fea-b49bc2359849
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 648766b45290fce3b954265b854c5bbd2fb0602c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351294"
---
# <a name="xml-format-files-sql-server"></a>Fichiers de format XML (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] fournit un schéma XML qui définit la syntaxe des *fichiers de format XML* à utiliser pour l'importation en bloc de données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les fichiers de format XML doivent respecter ce schéma, qui est défini en langage XSDL (XML Schema Definition Language). Les fichiers de format XML ne sont pris en charge que si les outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installés conjointement avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 Vous pouvez utiliser un fichier de format XML avec une commande **bcp**, une instruction BULK INSERT ou une instruction INSERT... Instruction SELECT \* FROM OPENROWSET(BULK...). La commande **bcp** vous permet de générer automatiquement un fichier de format XML pour une table. Pour plus d’informations, voir [bcp Utility](../../tools/bcp-utility.md).  
  
> [!NOTE]  
>  Deux types de fichiers de format sont pris en charge pour l’exportation et l’importation en bloc : les *fichiers de format non XML* et les *fichiers de format XML*. Les fichiers de format XML offrent une solution souple et puissante aux fichiers de format non XML. Pour plus d’informations sur les fichiers de format non-XML, consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
 **Dans cette rubrique :**  
  
-   [Avantages des fichiers de format XML](#BenefitsOfXmlFFs)  
  
-   [Structure des fichiers de format XML](#StructureOfXmlFFs)  
  
-   [Syntaxe de schéma pour les fichiers de format XML](#SchemaSyntax)  
  
-   [Exemples de fichiers de format XML](#SampleXmlFFs)  
  
-   [Tâches associées](#RelatedTasks)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="benefits-of-xml-format-files"></a><a name="BenefitsOfXmlFFs"></a> Avantages des fichiers de format XML  
  
-   Les fichiers de format XML sont autodescriptifs, ce qui en facilite la lecture, la création et l'extension. Ils sont lisibles, ce qui permet de facilement comprendre la manière dont les données sont interprétées lors des opérations en bloc.  
  
-   Les fichiers de format XML contiennent les types de données des colonnes cibles.  L'encodage XML décrit clairement les types de données et les éléments de données du fichier de données ainsi que la correspondance entre les éléments de données et les colonnes de table.  
  
     Ceci permet d'établir une séparation entre la manière dont les données sont représentées dans le fichier de données et le type de données associé à chaque champ du fichier. Par exemple, si un fichier de données contient une représentation en caractères des données, le type de colonne SQL correspondant est perdu.  
  
-   Un format de fichier XML permet de charger un champ qui contient un seul type de données LOB à partir d'un fichier de données.  
  
-   Un fichier de format XML peut être amélioré tout en demeurant compatible avec ses versions antérieures. En outre, la clarté de l'encodage XML facilite la création de plusieurs fichiers de format pour un fichier de données spécifique. Cela est utile si vous devez mapper tout ou partie des champs de données à des colonnes de différentes tables ou vues.  
  
-   La syntaxe XML ne dépend pas de la direction dans laquelle s'effectue l'opération : elle est la même qu'il s'agisse d'une exportation ou d'une importation en bloc.  
  
-   Vous pouvez utiliser les fichiers de format XML pour importer des données en bloc dans des tables ou dans des vues non partitionnées et pour exporter des données en bloc.  
  
-   Pour la fonction OPENROWSET (BULK…), il n'est pas obligatoire de spécifier une table cible. En effet, cette fonction s'appuie sur le fichier de format XML pour lire les données d'un fichier de données.  
  
    > [!NOTE]  
    >  Une table cible est nécessaire avec la commande **bcp** et l’instruction BULK INSERT, qui utilise les colonnes de la table cible pour effectuer la conversion de type.  
  
##  <a name="structure-of-xml-format-files"></a><a name="StructureOfXmlFFs"></a> Structure des fichiers de format XML  
 À l'image d'un fichier de format non-XML, un fichier de format XML définit le format et la structure des champs de données d'un fichier de données et mappe ces champs avec les colonnes d'une table cible spécifique.  
  
 Un fichier de format XML possède deux composants principaux, \<RECORD> et \<ROW> :  
  
-   Le composant \<RECORD> décrit les données telles qu’elles sont stockées dans le fichier de données.  
  
     Chaque élément \<RECORD> contient un ensemble d’un ou de plusieurs éléments \<FIELD>. Ces éléments correspondent aux champs du fichier de données. La syntaxe de base est la suivante :  
  
     \<RECORD>  
  
     \<FIELD .../> [ ...*n* ]  
  
     \</RECORD>  
  
     Chaque élément \<FIELD> décrit le contenu d’un champ de données spécifique. Un champ ne peut être mappé qu'à une seule colonne de la table. Tous les champs ne doivent pas être mappés à des colonnes.  
  
     Un champ d'un fichier de données peut être de longueur fixe/variable ou terminé par un caractère. Une *valeur de champ* peut être représentée par : un caractère (représentation sur un octet), un caractère large (représentation Unicode sur deux octets), un nom de fichier ou dans un format de base de données natif. Si une valeur de champ est représentée par un nom de fichier, celui-ci pointe vers le fichier qui contient la valeur d'une colonne BLOB de la table cible.  
  
-   Le composant \<ROW> explique comment créer des lignes de données à partir d’un fichier de données quand les données du fichier sont importées dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Un élément \<ROW> contient un ensemble d’éléments \<COLUMN>. Ces éléments correspondent à des colonnes de table. La syntaxe de base est la suivante :  
  
     \<ROW>  
  
     \<COLUMN .../> [ ...*n* ]  
  
     \</ROW>  
  
     Chaque élément \<COLUMN> ne peut être mappé qu’à un seul champ du fichier de données. L’ordre des éléments \<COLUMN> dans l’élément \<ROW> définit l’ordre dans lequel ils sont renvoyés par l’opération en bloc. Le fichier de format XML attribue à chaque élément \<COLUMN> un nom local qui n’a aucune relation avec la colonne dans la table cible d’une opération d’importation en bloc.  
  
##  <a name="schema-syntax-for-xml-format-files"></a><a name="SchemaSyntax"></a> Syntaxe de schéma pour les fichiers de format XML  
 Cette section contient un récapitulatif des éléments et des attributs du schéma XML des fichiers de format XML. La syntaxe d'un fichier de format ne dépend pas de la direction dans laquelle s'effectue l'opération : elle est la même qu'il s'agisse d'une exportation ou d'une importation en bloc. Cette section examine également comment l’importation en bloc utilise les éléments \<ROW> et \<COLUMN>, et comment inclure la valeur xsi:type d’un élément dans un jeu de données.  
  
 Pour voir comment la syntaxe correspond aux fichiers de format XML réels, consultez [Exemples de fichiers de format XML](#SampleXmlFFs), plus loin dans cette rubrique.  
  
> [!NOTE]  
>  Vous pouvez modifier un fichier de format de manière à effectuer une importation en bloc à partir d'un fichier de données dans lequel le nombre et/ou l'ordre des champs diffèrent du nombre et/ou de l'ordre des colonnes de données. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 **Dans cette section :**  
  
-   [Syntaxe de base du schéma XML](#BasicSyntax)  
  
-   [Comment l’importation en bloc utilise l’élément \<ROW>](#HowUsesROW)  
  
-   [Comment l’importation en bloc utilise l’élément \<COLUMN>](#HowUsesColumn)  
  
-   [Placement de la valeur xsi:type dans un ensemble de données](#PutXsiTypeValueIntoDataSet)  
  
###  <a name="basic-syntax-of-the-xml-schema"></a><a name="BasicSyntax"></a> Syntaxe de base du schéma XML  
 Ces instructions de syntaxe illustrent uniquement les éléments (\<BCPFORMAT>, \<RECORD>, \<FIELD>, \<ROW> et \<COLUMN>), ainsi que leurs attributs de base.  
  
 \<BCPFORMAT ...>  
  
 \<RECORD>  
  
 \<FIELD ID = "*fieldID*" xsi:type = "*fieldType*" [...]  
  
 />  
  
 \</RECORD>  
  
 \<ROW>  
  
 \<COLUMN SOURCE = "*fieldID*" NAME = "*columnName*" xsi:type = "*columnType*" [...]  
  
 />  
  
 \</ROW>  
  
 \</BCPFORMAT>  
  
> [!NOTE]  
>  Les attributs supplémentaires qui sont associés à la valeur de xsi:type dans un élément \<FIELD> ou \<COLUMN> sont décrits ultérieurement dans cette rubrique.  
  
 **Dans cette section :**  
  
-   [Éléments du schéma](#SchemaElements)  
  
-   [Attributs de l’élément \<FIELD>](#AttrOfFieldElement) (et [les valeurs Xsi:type de l’élément \<FIELD>](#XsiTypeValuesOfFIELD))  
  
-   [Attributs de l’élément \<COLUMN>](#AttrOfColumnElement) (et [les valeurs Xsi:type de l’élément \<COLUMN>](#XsiTypeValuesOfCOLUMN))  
  
####  <a name="schema-elements"></a><a name="SchemaElements"></a> Éléments du schéma  
 Cette section résume l'objet de chaque élément que le schéma XML définit pour les fichiers de format XML. Les attributs sont décrits dans des sections séparées plus loin dans cette rubrique.  
  
 \<BCPFORMAT>  
 Élément format-file qui définit la structure d'enregistrement d'un fichier de données spécifique et sa correspondance aux colonnes d'une ligne dans la table.  
  
 \<RECORD .../>  
 Définit un élément complexe contenant un ou plusieurs éléments \<FIELD>. L'ordre dans lequel les champs sont déclarés dans le fichier de format est celui dans lequel ces champs apparaissent dans le fichier de données.  
  
 \<FIELD .../>  
 Définit un champ dans le fichier de données, qui contient des données.  
  
 Les attributs de cet élément sont décrits dans la section [Attributs de l’élément \<FIELD>](#AttrOfFieldElement), plus loin dans cette rubrique.  
  
 \<ROW .../>  
 Définit un élément complexe contenant un ou plusieurs éléments \<COLUMN>. L’ordre des éléments \<COLUMN> est indépendant de l’ordre des éléments \<FIELD> dans une définition RECORD. En revanche, l’ordre des éléments \<COLUMN> dans un fichier de format détermine l’ordre des colonnes de l’ensemble de lignes résultant. Les champs de données sont chargés dans l’ordre de déclaration des éléments \<COLUMN> correspondants dans l’élément \<COLUMN>.  
  
 Pour plus d’informations, consultez [Comment l’importation en bloc utilise l’élément \<ROW>](#HowUsesROW), plus loin dans cette rubrique.  
  
 \<COLUMN>  
 Définit une colonne comme élément (\<COLUMN>). Chaque élément \<COLUMN> correspond à un élément \<FIELD> (dont l’ID est spécifié dans l’attribut SOURCE de l’élément \<COLUMN>).  
  
 Les attributs de cet élément sont décrits dans la section [Attributs de l’élément \<COLUMN>](#AttrOfColumnElement), plus loin dans cette rubrique. Consultez également [Comment l’importation en bloc utilise l’élément \<COLUMN>](#HowUsesColumn), plus loin dans cette rubrique.  
  
 \</BCPFORMAT>  
 Requis pour terminer le fichier de format.  
  
####  <a name="attributes-of-the-field-element"></a><a name="AttrOfFieldElement"></a> Attributs de l'élément \<FIELD>  
 Cette section décrit les attributs de l’élément \<FIELD>, qui sont récapitulés dans la syntaxe de schéma suivante :  
  
 <FIELD  
  
 ID **="** _fieldID_ **"**  
  
 xsi **:** type **="** _fieldType_ **"**  
  
 [ LENGTH **="** _n_ **"** ]  
  
 [ PREFIX_LENGTH **="** _p_ **"** ]  
  
 [ MAX_LENGTH **="** _m_ **"** ]  
  
 [ COLLATION **="** _collationName_ **"** ]  
  
 [ TERMINATOR **="** _terminator_ **"** ]  
  
 />  
  
 Chaque élément \<FIELD> est indépendant des autres. Un champ est décrit en termes des attributs suivants :  
  
|Attribut FIELD|Description|Facultatif /<br /><br /> Obligatoire|  
|---------------------|-----------------|------------------------------|  
|ID **="** _fieldID_ **"**|Spécifie le nom logique du champ dans le fichier de données. L'ID d'un champ est la clé utilisée pour y faire référence.<br /><br /> \<FIELD ID**="**_fieldID_**"**/> est mappé à \<COLUMN SOURCE**="**_fieldID_**"**/>|Obligatoire|  
|xsi:type **="** _fieldType_ **"**|Il s'agit d'une construction XML (utilisée comme un attribut) qui identifie le type de l'instance de l'élément. La valeur de *fieldType* détermine de quels attributs facultatifs (ci-dessous) vous avez besoin dans une instance donnée.|Obligatoire (selon le type de données)|  
|LENGTH **="** _n_ **"**|Cet attribut définit la longueur pour une instance d'un type de données à longueur fixe.<br /><br /> Cette valeur de *n* doit être un entier positif.|Facultatif sauf s'il est requis par la valeur xsi:type|  
|PREFIX_LENGTH **="** _p_ **"**|Cet attribut définit la longueur de préfixe pour une représentation de données binaires. La valeur PREFIX_LENGTH, *p*, doit correspondre à l’une des valeurs suivantes : 1, 2, 4 ou 8.|Facultatif sauf s'il est requis par la valeur xsi:type|  
|MAX_LENGTH **="** _m_ **"**|Cet attribut est le nombre maximal d'octets pouvant être stockés dans un champ donné. Sans table cible, la longueur maximale de la colonne est inconnue. L'attribut MAX_LENGTH limite la longueur maximale d'une colonne de caractères en sortie, limitant ainsi le stockage alloué pour la valeur de la colonne. Ceci est particulièrement pratique lors de l'utilisation de l'option BULK de la fonction OPENROWSET dans une clause SELECT FROM.<br /><br /> Cette valeur de *m* doit être un entier positif. Par défaut, la longueur maximale est de 8 000 caractères pour une colonne **char** et de 4 000 caractères pour une colonne **nchar** .|Facultatif|  
|COLLATION **="** _collationName_ **"**|COLLATION est uniquement autorisé pour les champs caractères. Pour obtenir la liste des noms du classement SQL, consultez [Nom du classement SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).|Facultatif|  
|TERMINATOR **= "** _terminator_ **"**|Cet attribut spécifie la marque de fin d'un champ de données. La marque de fin peut être n'importe quel caractère. La marque de fin doit être un caractère unique ne faisant pas partie des données.<br /><br /> Par défaut, la marque de fin de champ est le caractère tabulation (représenté par \t). Pour représenter une marque de paragraphe, utilisez \r\n.|Utilisé uniquement avec un xsi:type de données caractères, qui nécessite cet attribut|  
  
#####  <a name="xsitype-values-of-the-field-element"></a><a name="XsiTypeValuesOfFIELD"></a> Valeurs Xsi:type de l’élément \<FIELD>  
 La valeur xsi:type est une construction XML (utilisée comme un attribut) qui identifie le type de données d'une instance d'un élément. Pour plus d'informations sur l'utilisation de cette valeur, consultez « Placement de la valeur xsi:type dans un ensemble de données », plus loin dans cette section.  
  
 La valeur xsi:type de l’élément \<FIELD> prend en charge les types de données suivants.  
  
|Valeurs xsi:type \<FIELD>|Attribut(s) XML requis<br /><br /> pour le type de données|Attribut(s) XML facultatif(s)<br /><br /> pour le type de données|  
|-------------------------------|---------------------------------------------------|---------------------------------------------------|  
|**NativeFixed**|**LENGTH**|Aucun.|  
|**NativePrefix**|**PREFIX_LENGTH**|MAX_LENGTH|  
|**CharFixed**|**LENGTH**|COLLATION|  
|**NCharFixed**|**LENGTH**|COLLATION|  
|**CharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH, COLLATION|  
|**NCharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH, COLLATION|  
|**CharTerm**|**TERMINATOR**|MAX_LENGTH, COLLATION|  
|**NCharTerm**|**TERMINATOR**|MAX_LENGTH, COLLATION|  
  
 Pour plus d’informations sur les types de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
####  <a name="attributes-of-the-column-element"></a><a name="AttrOfColumnElement"></a> Attributs de l'élément \<COLUMN>  
 Cette section décrit les attributs de l’élément \<COLUMN>, qui sont récapitulés dans la syntaxe de schéma suivante :  
  
 <COLUMN  
  
 SOURCE = "*fieldID*"  
  
 NAME = "*columnName*"  
  
 xsi:type = "*columnType*"  
  
 [ LENGTH = "*n*" ]  
  
 [ PRECISION = "*n*" ]  
  
 [ SCALE = "*value*" ]  
  
 [ NULLABLE = { "YES"  
  
 "NO" } ]  
  
 />  
  
 Un champ est mappé à une colonne dans la table cible à l'aide des attributs suivants :  
  
|Attribut COLUMN|Description|Facultatif /<br /><br /> Obligatoire|  
|----------------------|-----------------|------------------------------|  
|SOURCE **="** _fieldID_ **"**|Spécifie l'ID du champ mappé à la colonne.<br /><br /> \<COLUMN SOURCE**="**_fieldID_**"**/> est mappé à \<FIELD ID**="**_fieldID_**"**/>|Obligatoire|  
|NAME = "*columnName*"|Spécifie le nom de la colonne dans l'ensemble de lignes représenté par le fichier de format. Ce nom de colonne est utilisé pour identifier la colonne dans le jeu de résultats, et il ne doit pas nécessairement correspondre au nom de colonne utilisé dans la table cible.|Obligatoire|  
|xsi **:** type **="** _ColumnType_ **"**|Il s'agit d'une construction XML (utilisée comme un attribut) qui identifie le type de données de l'instance de l'élément. La valeur de *ColumnType* détermine de quels attributs facultatifs (ci-dessous) vous avez besoin dans une instance donnée.<br /><br /> Remarque : Les valeurs possibles de *ColumnType* et leurs attributs associés sont répertoriés dans le tableau relatif à l’élément \<COLUMN> dans la section [Valeurs Xsi:type de l’élément &lt;&gt;](#XsiTypeValuesOfCOLUMN).|Facultatif|  
|LENGTH **="** _n_ **"**|Définit la longueur d'une instance d'un type de données à longueur fixe. LENGTH est utilisé uniquement lorsque xsi:type est un type de données string.<br /><br /> Cette valeur de *n* doit être un entier positif.|Facultatif (disponible uniquement si xsi:type est un type de données string)|  
|PRECISION **="** _n_ **"**|Nombre de chiffres qui composent un nombre. Par exemple, le nombre 123,45 a une précision de 5.<br /><br /> Cette valeur doit être un entier positif.|Facultatif (disponible uniquement si xsi:type est un type de données variable-number)|  
|SCALE **="** _int_ **"**|Indique le nombre de chiffres à droite du point décimal (notre virgule) dans un nombre. Par exemple, le nombre 123,45 a une précision de 2.<br /><br /> La valeur doit être un entier.|Facultatif (disponible uniquement si xsi:type est un type de données variable-number)|  
|NULLABLE **=** { **"** YES **"**<br /><br /> **"** NO **"** }|Indique si une colonne peut prendre des valeurs NULL. Cet attribut est complètement indépendant de FIELDS. Cependant, si une colonne n'est pas NULLABLE et si le champ spécifie NULL (en ne spécifiant pas de valeur), une erreur d'exécution en résulte.<br /><br /> L'attribut NULLABLE est utilisé uniquement si vous effectuez une instruction SELECT FROM OPENROWSET(BULK...) ordinaire.|Facultatif (disponible pour n'importe quel type de données)|  
  
#####  <a name="xsitype-values-of-the-column-element"></a><a name="XsiTypeValuesOfCOLUMN"></a> Valeurs Xsi:type de l’élément \<COLUMN>  
 La valeur xsi:type est une construction XML (utilisée comme un attribut) qui identifie le type de données d'une instance d'un élément. Pour plus d'informations sur l'utilisation de cette valeur, consultez « Placement de la valeur xsi:type dans un ensemble de données », plus loin dans cette section.  
  
 L’élément \<COLUMN> prend en charge les types de données SQL natifs, comme suit :  
  
|Catégorie de type|Types de données \<COLUMN>|Attribut(s) XML requis<br /><br /> pour le type de données|Attribut(s) XML facultatif(s)<br /><br /> pour le type de données|  
|-------------------|---------------------------|---------------------------------------------------|---------------------------------------------------|  
|Résolution|**SQLBIT**, **SQLTINYINT**, **SQLSMALLINT**, **SQLINT**, **SQLBIGINT**, **SQLFLT4**, **SQLFLT8**, **SQLDATETIME**, **SQLDATETIM4**, **SQLDATETIM8**, **SQLMONEY**, **SQLMONEY4**, **SQLVARIANT** et **SQLUNIQUEID**|Aucun.|NULLABLE|  
|Nombre variable|**SQLDECIMAL** et **SQLNUMERIC**|Aucun.|NULLABLE, PRECISION, SCALE|  
|LOB|**SQLIMAGE**, **CharLOB**, **SQLTEXT** et **SQLUDT**|Aucun.|NULLABLE|  
|LOB caractère|**SQLNTEXT**|Aucun.|NULLABLE|  
|Chaîne binaire|**SQLBINARY** et **SQLVARYBIN**|Aucun.|NULLABLE, LENGTH|  
|Chaîne de caractères|**SQLCHAR**, **SQLVARYCHAR**, **SQLNCHAR** et **SQLNVARCHAR**|Aucun.|NULLABLE, LENGTH|  
  
> [!IMPORTANT]  
>  Pour exporter ou importer en bloc des données SQLXML, utilisez l'un des types de données ci-dessous dans votre fichier de format : SQLCHAR ou SQLVARYCHAR (les données sont envoyées dans la page de codes client ou dans la page de codes inhérente au classement) ; SQLNCHAR ou SQLNVARCHAR (les données sont envoyées au format Unicode) ; SQLBINARY ou SQLVARYBIN (les données sont envoyées sans être converties).  
  
 Pour plus d’informations sur les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
###  <a name="how-bulk-import-uses-the-row-element"></a><a name="HowUsesROW"></a> Comment l’importation en bloc utilise l’élément \<ROW>  
 L’élément \<ROW> est ignoré dans certains contextes. L’éventuelle incidence d’un élément \<ROW> sur une opération d’importation en bloc dépend du mode d’exécution de l’opération :  
  
-   Commande **bcp**  
  
     Quand des données sont chargées dans une table cible, **bcp** ignore le composant \<ROW>. **bcp** charge plutôt les données en fonction des types de colonnes de la table cible.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] Instructions (fournisseur d’ensembles de lignes BULK INSERT et OPENROWSET)  
  
     Lors de l’importation en bloc de données dans une table, les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisent le composant \<ROW> pour générer l’ensemble de lignes d’entrée. En outre, les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] effectuent les conversions de type appropriées en fonction des types de colonnes spécifiés sous \<ROW> et de la colonne correspondante dans la table cible. Si une discordance existe entre les types de colonnes spécifiés dans le fichier de format et dans la table cible, une conversion de type supplémentaire intervient. Cette conversion de type supplémentaire peut entraîner certaines différences (c’est-à-dire, une perte de précision) dans le comportement du fournisseur d’ensembles de lignes en bloc BULK INSERT ou OPENROWSET en comparaison de **bcp**.  
  
     Les informations de l’élément \<ROW> permettent de construire une ligne sans nécessiter d’informations supplémentaires. Pour cette raison, vous pouvez générer un ensemble de lignes en utilisant l’instruction SELECT (SELECT \* FROM OPENROWSET(BULK *datafile* FORMATFILE=*xmlformatfile*).  
  
    > [!NOTE]  
    >  La clause OPENROWSET BULK nécessite un fichier de format (notez que la conversion du type de données du champ au type de données d'une colonne n'est possible qu'avec un fichier de format XML).  
  
###  <a name="how-bulk-import-uses-the-column-element"></a><a name="HowUsesColumn"></a> Comment l’importation en bloc utilise l’élément \<COLUMN>  
 Pour l’importation en bloc de données dans une table, les éléments \<COLUMN> dans un fichier de format mappent un champ de fichier de données à des colonnes de table en spécifiant les points suivants :  
  
-   La position de chaque champ dans une ligne du fichier de données.  
  
-   Le type de colonne, qui est utilisé pour convertir le type de données du champ au type de données de la colonne souhaitée.  
  
 Si aucune colonne n'est mappée à un champ, le champ n'est pas copié dans la ou les lignes générées. Ce comportement permet à un fichier de données de générer des lignes avec différentes colonnes (dans différentes tables).  
  
 De même, pour l’exportation en bloc de données à partir d’une table, chaque élément \<COLUMN> du fichier de format mappe la colonne de la ligne de la table d’entrée à son champ correspondant du fichier de données de sortie.  
  
###  <a name="putting-the-xsitype-value-into-a-data-set"></a><a name="PutXsiTypeValueIntoDataSet"></a> Placement de la valeur xsi:type dans un ensemble de données  
 Lorsqu'un document XML est validé à travers le langage XSD (XML Schema Definition), la valeur xsi:type n'est pas placée dans l'ensemble de données. Cependant, vous pouvez placer les informations xsi:type dans l'ensemble de données en chargeant le fichier de format XML dans un document XML (par exemple, `myDoc`), tel qu'illustré dans l'extrait de code suivant :  
  
```cs
...;  
myDoc.LoadXml(xmlFormat);  
XmlNodeList ColumnList = myDoc.GetElementsByTagName("COLUMN");  
for(int i=0;i<ColumnList.Count;i++)  
{  
   Console.Write("COLUMN: xsi:type=" +ColumnList[i].Attributes["type",  
      "http://www.w3.org/2001/XMLSchema-instance"].Value+"\n");  
}  
```  
  
##  <a name="sample-xml-format-files"></a><a name="SampleXmlFFs"></a> Exemples de fichiers de format XML  
 Cette section contient des informations sur l'utilisation de fichiers de format XML dans différents cas de figure, avec un exemple [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] .  
  
> [!NOTE]  
>  Dans les fichiers de données illustrés dans les exemples ci-dessous, `<tab>` indique un caractère de tabulation et `<return>` , un retour chariot.  
  
 Ces exemples illustrent les aspects essentiels de l'utilisation de fichiers de format XML :  
  
-   [Classement de champs de données caractères dans le même ordre que les colonnes d'une table](#OrderCharFieldsSameAsCols)  
  
-   [Classement de champs de données et de colonnes d'une table dans un ordre différent](#OrderFieldsAndColsDifferently)  
  
-   [Omission d'un champ de données](#OmitField)  
  
-   [Mappage de différents types de champs sur des colonnes](#MapXSItype)  
  
-   [Mappage de données XML sur une table](#MapXMLDataToTbl)  
  
-   [Importation de champs à longueur fixe ou à largeur fixe](#ImportFixedFields)  
  
-   [Autre exemple](#AdditionalExamples)  
  
> [!NOTE]  
>  Pour plus d’informations sur la création de fichiers de format, consultez [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
###  <a name="a-ordering-character-data-fields-the-same-as-table-columns"></a><a name="OrderCharFieldsSameAsCols"></a> A. Classement de champs de données caractères dans le même ordre que les colonnes d'une table  
 L'exemple suivant illustre un fichier de format XML décrivant un fichier de données qui contient trois champs de données caractères. Le fichier de format mappe le fichier de données sur une table contenant trois colonnes. Les champs de données correspondent un-à-un aux colonnes de la table.  
  
 **Table (ligne) :** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **Fichier de données (enregistrement) :** Age\<tab>Firstname\<tab>Lastname\<return>  
  
 Le fichier de format XML suivant lit les données du fichier de données et les écrit dans la table.  
  
 Dans l'élément `<RECORD>` , le fichier de format représente les valeurs de données de chacun des trois champs au format caractère. Pour chaque champ, l'attribut `TERMINATOR` indique le terminateur qui suit la valeur de données.  
  
 Les champs de données correspondent un-à-un aux colonnes de la table. Dans l'élément `<ROW>` , le fichier de format mappe la colonne `Age` au premier champ, la colonne `FirstName` au deuxième et la colonne `LastName` au troisième.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>   
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="2" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="3" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Pour obtenir un exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] équivalent, consultez [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
###  <a name="b-ordering-data-fields-and-table-columns-differently"></a><a name="OrderFieldsAndColsDifferently"></a> B. Classement de champs de données et de colonnes d'une table dans un ordre différent  
 L'exemple suivant illustre un fichier de format XML décrivant un fichier de données qui contient trois champs de données caractères. Le fichier de format mappe le fichier de données sur une table contenant trois colonnes classées dans un ordre différent de celui des champs du fichier de données.  
  
 **Table (ligne) :** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **Fichier de données** (enregistrement) : Age\<tab>Lastname\<tab>Firstname\<return>  
  
 Dans l'élément `<RECORD>` , le fichier de format représente les valeurs de données de chacun des trois champs au format caractère.  
  
 Dans l'élément `<ROW>` , le fichier de format mappe la colonne `Age` au premier champ, la colonne `FirstName` au troisième et la colonne `LastName` au second.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="2" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Pour obtenir un exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] équivalent, consultez [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md).  
  
###  <a name="c-omitting-a-data-field"></a><a name="OmitField"></a> C. Omission d'un champ de données  
 L'exemple suivant illustre un fichier de format XML décrivant un fichier de données qui contient quatre champs de données caractères. Le fichier de format mappe le fichier de données sur une table contenant trois colonnes. Le deuxième champ de données ne correspond à aucune colonne de la table.  
  
 **Table (ligne) :** Person (Age int, FirstName Varchar(20), LastName Varchar(30))  
  
 **Fichier de données (enregistrement) :** Age\<tab>employeeID\<tab>Firstname\<tab>Lastname\<return>  
  
 Dans l'élément `<RECORD>` , le fichier de format représente les valeurs de données de chacun des quatre champs au format caractère. Pour chaque champ, l'attribut TERMINATOR indique le terminateur qui suit la valeur de données.  
  
 Dans l'élément `<ROW>` , le fichier de format mappe la colonne `Age` au premier champ, la colonne `FirstName` au troisième et la colonne `LastName` au quatrième.  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="10"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="4" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Pour obtenir un exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] équivalent, consultez [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md).  
  
###  <a name="d-mapping-field-xsitype-to-column-xsitype"></a><a name="MapXSItype"></a> D. Mappage de xsi:type \<FIELD> à xsi:type \<COLUMN>  
 L'exemple suivant illustre différents types de champs et leurs mappages sur des colonnes.  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT  
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <RECORD>  
      <FIELD xsi:type="CharTerm" ID="C1" TERMINATOR="\t"   
            MAX_LENGTH="4"/>  
      <FIELD xsi:type="CharFixed" ID="C2" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="CharPrefix" ID="C3" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharTerm" ID="C4" TERMINATOR="\t"   
         MAX_LENGTH="4"/>  
      <FIELD xsi:type="NCharFixed" ID="C5" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharPrefix" ID="C6" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NativeFixed" ID="C7" LENGTH="4"/>  
   </RECORD>  
   <ROW>  
      <COLUMN SOURCE="C1" NAME="Age" xsi:type="SQLTINYINT"/>  
      <COLUMN SOURCE="C2" NAME="FirstName" xsi:type="SQLVARYCHAR"   
      LENGTH="16" NULLABLE="NO"/>  
      <COLUMN SOURCE="C3" NAME="LastName" />  
      <COLUMN SOURCE="C4" NAME="Salary" xsi:type="SQLMONEY"/>  
      <COLUMN SOURCE="C5" NAME="Picture" xsi:type="SQLIMAGE"/>  
      <COLUMN SOURCE="C6" NAME="Bio" xsi:type="SQLTEXT"/>  
      <COLUMN SOURCE="C7" NAME="Interest"xsi:type="SQLDECIMAL"   
      PRECISION="5" SCALE="3"/>  
   </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="e-mapping-xml-data-to-a-table"></a><a name="MapXMLDataToTbl"></a> E. Mappage de données XML sur une table  
 Dans l'exemple ci-dessous, une table (`t_xml`) vide à deux colonnes est créée : sa première colonne est mappée sur le type de données `int` et la seconde, sur le type de données `xml` .  
  
```sql
CREATE TABLE t_xml (c1 int, c2 xml)  
```  
  
 Le fichier de format XML suivant charge un fichier de données dans la table `t_xml`.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativePrefix" PREFIX_LENGTH="1"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="c1" xsi:type="SQLINT"/>  
  <COLUMN SOURCE="2" NAME="c2" xsi:type="SQLNCHAR"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="f-importing-fixed-length-or-fixed-width-fields"></a><a name="ImportFixedFields"></a> F. Importation de champs à longueur fixe ou à largeur fixe  
 L'exemple suivant décrit des champs fixes de `10` ou `6` caractères chacun. Le fichier de format représente ces longueurs-largeurs de champ sous la forme de `LENGTH="10"` et `LENGTH="6"`, respectivement. Chaque ligne des fichiers de données se termine par une combinaison retour chariot/saut de ligne, {CR}{LF}, représentée par le fichier de format sous la forme `TERMINATOR="\r\n"`.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT  
       xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharFixed" LENGTH="10"/>  
    <FIELD ID="2" xsi:type="CharFixed" LENGTH="6"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="C1" xsi:type="SQLINT" />  
    <COLUMN SOURCE="2" NAME="C2" xsi:type="SQLINT" />  
  </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="additional-examples"></a><a name="AdditionalExamples"></a> Autres exemples  
 Les rubriques suivantes contiennent des exemples supplémentaires de fichiers de format non XML et de fichiers de format XML :  
  
-   [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
 Aucun.  
  
## <a name="see-also"></a>Voir aussi  
 [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
  
