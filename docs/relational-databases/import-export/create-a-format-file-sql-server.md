---
title: Créer un fichier de format (SQL Server) | Microsoft Docs
description: Quand vous importez ou exportez en bloc une table SQL Server, un fichier de format permet d’écrire des fichiers de données avec peu d’édition ou de lire des fichiers de données provenant d’autres programmes.
ms.custom: ''
ms.date: 02/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], creating
ms.assetid: f680b4a0-630f-4052-9c79-d348c1076f7b
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 12e75389d7e5878107f7efe5f4fe88494354cf1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351370"
---
# <a name="create-a-format-file-sql-server"></a>Créer un fichier de format (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Lorsque vous importez des données en bloc dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou exportez en bloc des données à partir d'une table, utilisez un fichier de format vers un système souple pour l'écriture de fichiers de données nécessitant peu ou pas d'édition pour la conformité aux autres formats de données ou pour la lecture des fichiers de données provenant d'autres logiciels.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge deux types de fichier de format : format XML et format non-XML. Le format non-XML est le format d'origine pris en charge dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 En règle générale, les fichiers de format XML et non-XML sont interchangeables. Toutefois, nous recommandons d'utiliser la syntaxe XML pour les nouveaux fichiers de format, car elle offre plusieurs avantages par rapport aux fichiers de format non-XML.  
  
> [!NOTE]  
>  La version de l’utilitaire **bcp** (Bcp.exe) servant à lire un fichier de format doit être identique ou ultérieure à la version utilisée pour créer le fichier de format. Par exemple, **bcp** de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] peut lire un fichier de format version 10.0 généré par **bcp** de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], mais **bcp** de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ne peut pas lire un fichier de format version 11.0 généré par **bcp** de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 Cette rubrique décrit l'utilisation de l' [utilitaire bcp](../../tools/bcp-utility.md) pour créer un fichier de format pour une table donnée. Le fichier de format est basé sur l’option de type de données spécifiée ( **-n**, **-c**, **-w** ou **-N**) et les délimiteurs de la table ou de la vue.  
  
## <a name="creating-a-non-xml-format-file"></a>Création d'un fichier de format non-XML  
 Pour utiliser une commande **bcp** pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données. L’option **format** requiert également l’option **-f** , comme suit :  
  
 **bcp**_table_ou_vue_**format** nul **-f**_nom_fichier_de_format_  
  
> [!NOTE]  
> Pour bien distinguer un fichier au format non-XML, nous vous recommandons d'utiliser l'extension de nom de fichier .fmt, par exemple MaTable.fmt.  
  
 Pour plus d’informations sur la structure et les champs des fichiers de format non-XML, consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
### <a name="examples"></a>Exemples  
 Cette section contient les exemples suivants qui illustrent l’utilisation des commandes **bcp** pour créer un fichier de format non-XML :  
  
-   R. Création d'un fichier de format non-XML pour des données natives  
  
-   B. Création d'un fichier de format non-XML pour des données de type caractère  
  
-   C. Création d'un fichier de format non-XML pour des données Unicode natives  
  
-   D. Création d'un fichier de format non-XML pour des données Unicode de type caractère  
  
-   F. Utilisation d’un fichier de format avec l’option de page de code  
  
 Ces exemples créent la table `HumanResources.Department` dans l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La table `HumanResources.Department` contient quatre colonnes : `DepartmentID`, `Name`, `GroupName`et `ModifiedDate`.  
  
#### <a name="a-creating-a-non-xml-format-file-for-native-data"></a>R. Création d'un fichier de format non-XML pour des données natives  
 L'exemple suivant crée un fichier de format XML appelé `Department-n.xml`et destiné à la table [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Le fichier de format utilise des types de données natives. Le contenu du fichier ainsi généré vous est présenté après la commande.  
  
 La commande **bcp** contient les qualificateurs suivants.  
  
|Qualificateurs|Description|  
|----------------|-----------------|  
|**formatnul-f** _format_file_|Format de fichier non-XML.|  
|**-n**|Spécifie les types de données natifs.|  
|**-T**|Spécifie que l'utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec une connexion approuvée qui utilise la sécurité intégrée. Si **-T** n’est pas spécifié, vous devez indiquer **-U** et **-P** pour pouvoir vous connecter.|  
  
 Dans la fenêtre d'invite de commandes Windows, tapez la commande `bcp` suivante :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -n -f Department-n.fmt  
```  
  
 Le fichier de format généré, `Department-n.fmt`, contient les informations suivantes :  
  
```  
12.0  
4  
1  SQLSMALLINT   0       2       ""   1     DepartmentID         ""  
2  SQLNCHAR      2       100     ""   2     Name                 SQL_Latin1_General_CP1_CI_AS  
3  SQLNCHAR      2       100     ""   3     GroupName            SQL_Latin1_General_CP1_CI_AS  
4  SQLDATETIME   0       8       ""   4     ModifiedDate         ""  
```  
  
 Pour plus d’informations, consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
#### <a name="b-creating-a-non-xml-format-file-for-character-data"></a>B. Création d'un fichier de format non-XML pour des données de type caractère  
 L'exemple suivant crée un fichier de format XML appelé `Department.fmt`et destiné à la table [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Le fichier de format utilise les formats de données de type caractère ainsi qu'un indicateur de fin de champ qui n'est pas défini par défaut (`,`). Le contenu du fichier ainsi généré vous est présenté après la commande.  
  
 La commande **bcp** contient les qualificateurs suivants.  
  
|Qualificateurs|Description|  
|----------------|-----------------|  
|**formatnul-f** _format_file_|Format de fichier non-XML.|  
|**-c**|Données de type caractère.|  
|**-T**|Spécifie que l'utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec une connexion approuvée qui utilise la sécurité intégrée. Si **-T** n’est pas spécifié, vous devez indiquer **-U** et **-P** pour pouvoir vous connecter.|  
  
 Dans la fenêtre d'invite de commandes Windows, tapez la commande `bcp` suivante :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -f Department-c.fmt -T  
```  
  
 Le fichier de format généré, `Department-c.fmt`, contient les informations suivantes :  
  
```  
12.0  
4  
1  SQLCHAR       0       7       "\t"     1     DepartmentID            ""  
2  SQLCHAR       0       100     "\t"     2     Name                    SQL_Latin1_General_CP1_CI_AS  
3  SQLCHAR       0       100     "\t"     3     GroupName               SQL_Latin1_General_CP1_CI_AS  
4  SQLCHAR       0       24      "\r\n"   4     ModifiedDate            ""  
```  
  
 Pour plus d’informations, consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
#### <a name="c-creating-a-non-xml-format-file-for-unicode-native-data"></a>C. Création d'un fichier de format non-XML pour des données Unicode natives  
 Pour créer un fichier de format non-XML pour des données Unicode natives destinées à la table `HumanResources.Department`, utilisez la commande suivante :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -N -f Department-n.fmt  
```  
  
 Pour plus d’informations sur l’utilisation des données natives Unicode, consultez [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="d-creating-a-non-xml-format-file-for-unicode-character-data"></a>D. Création d'un fichier de format non-XML pour des données Unicode de type caractère  
 Pour créer un fichier de format non-XML pour des données Unicode de type caractère, s'appuyant sur des indicateurs de fin définis par défaut et destinées à la table `HumanResources.Department`, utilisez la commande suivante :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -w -f Department-w.fmt  
```  
  
 Pour plus d’informations sur l’utilisation des données de caractères Unicode, consultez [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="f-using-a-format-file-with-the-code-page-option"></a>F. Utilisation d’un fichier de format avec l’option de page de code  
Si vous créez un fichier de format à l’aide de la commande bcp (c’est-à-dire, en utilisant `bcp format`), les informations sur la page de code/classement sont écrites dans le fichier de format.   
L’exemple de fichier de format pour table avec 5 colonnes suivant inclut le classement.  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0          Cyrillic_General_CS_AS  
2  SQLCHAR         0       0       "**\t**"         2     c_1          Cyrillic_General_CS_AS  
3  SQLCHAR         0       3000    "**\t**"         3     c_2          Cyrillic_General_CS_AS  
4  SQLCHAR         0       5       "**\t**"         4     c_3          ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4          ""  
  
```  
  
 Si vous essayez d’importer des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de `bcp in -c -C65001 -f format_file` ... » ou de « `BULK INSERT`/`OPENROWSET` ... `FORMATFILE='format_file' CODEPAGE=65001` ... », les informations sur la page de code/classement ont la priorité sur l’option 65001.  
Par conséquent, si vous générez un fichier de format, vous devez supprimer manuellement les informations de classement du fichier de format généré avant de commencer l’importation des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
Voici un exemple de fichier de format sans les informations de classement.  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0              ""  
2  SQLCHAR         0       0       "**\t**"         2     c_1              ""  
3  SQLCHAR         0       3000    "**\t**"         3     c_2              ""  
4  SQLCHAR         0       5       "**\t**"         4     c_3              ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4              ""  
```  
  
## <a name="creating-an-xml-format-file"></a>Création d'un fichier de format XML  
 Pour utiliser une commande **bcp** pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données. L’option **format** nécessite toujours l’option **-f** ; la création d’un fichier de format XML nécessite également l’option **-x** , comme suit :  
  
 **bcp** _table_ou_vue_ **format nul-f** _nom_fichier_de_format_ **-x**  
  
> [!NOTE]  
> Pour bien distinguer un fichier de format XML, nous vous recommandons d'utiliser l'extension de nom de fichier .xml, par exemple MaTable.xml.  
  
 Pour plus d’informations sur la structure et les champs des fichiers de format XML, consultez [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).  
  
### <a name="examples"></a>Exemples  
 Cette section contient les exemples suivants, qui illustrent l’utilisation des commandes **bcp** pour créer un fichier de format XML :  
  
-   R. Création d'un fichier de format XML pour des données de type caractère  
-   B. Création d'un fichier de format XML pour des données natives  
  
 Ces exemples créent la table `HumanResources.Department` dans l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La table `HumanResources.Department` contient quatre colonnes : `DepartmentID`, `Name`, `GroupName`et `ModifiedDate`.  
  
> [!NOTE]  
>  [!INCLUDE[ssSampleDBdesc](../../includes/sssampledbdesc-md.md)]  
  
#### <a name="a-creating-an-xml-format-file-for-character-data"></a>R. Création d'un fichier de format XML pour des données de type caractère  
 L'exemple suivant crée un fichier de format XML appelé `Department.xml`et destiné à la table [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Le fichier de format utilise les formats de données de type caractère ainsi qu'un indicateur de fin de champ qui n'est pas défini par défaut (`,`). Le contenu du fichier ainsi généré vous est présenté après la commande.  
  
 La commande **bcp** contient les qualificateurs suivants.  
  
|Qualificateurs|Description|  
|----------------|-----------------|  
|**formatnul-f** _format_file_ **-x**|Fichier de format XML.|  
|**-c**|Données de type caractère.|  
|**-t** `,`|Virgule ( **,** ) servant d’indicateur de fin de champ.<br /><br /> Remarque : Si le fichier de données utilise l’indicateur de fin de champ défini par défaut (à savoir`\t`), le commutateur **-t** n’est pas nécessaire.|  
|**-T**|Spécifie que l'utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec une connexion approuvée qui utilise la sécurité intégrée. Si **-T** n’est pas spécifié, vous devez indiquer **-U** et **-P** pour pouvoir vous connecter.|  
  
 Dans la fenêtre d'invite de commandes Windows, tapez la commande `bcp` suivante :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -x -f Department-c.xml -t, -T  
```  
  
 Le fichier de format généré `Department-c.xml`contient les éléments XML suivants :  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>  
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="24"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 Pour obtenir des informations sur la syntaxe de ce fichier de format, consultez [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). Pour plus d’informations sur les données de type caractère, consultez [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="b-creating-an-xml-format-file-for-native-data"></a>B. Création d'un fichier de format XML pour des données natives  
 L'exemple suivant crée un fichier de format XML appelé `Department-n.xml`et destiné à la table `HumanResources.Department` . Le fichier de format utilise des types de données natives. Le contenu du fichier ainsi généré vous est présenté après la commande.  
  
 La commande **bcp** contient les qualificateurs suivants.  
  
|Qualificateurs|Description|  
|----------------|-----------------|  
|**formatnul-f** _format_file_ **-x**|Fichier de format XML.|  
|**-n**|Spécifie les types de données natifs.|  
|**-T**|Spécifie que l'utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec une connexion approuvée qui utilise la sécurité intégrée. Si **-T** n’est pas spécifié, vous devez indiquer **-U** et **-P** pour pouvoir vous connecter.|  
  
 Dans la fenêtre d'invite de commandes Windows, tapez la commande `bcp` suivante :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -x -f Department-n.xml -n -T  
```  
  
 Le fichier de format généré `Department-n.xml`contient les éléments XML suivants :  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativeFixed" LENGTH="2"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="NativeFixed" LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 Pour obtenir des informations sur la syntaxe de ce fichier de format, consultez [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). Pour plus d’informations sur les données de type caractère, consultez [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).  
  
## <a name="mapping-data-fields-to-table-columns"></a>Mappage des champs de données aux colonnes de table  
 Lorsqu’il est créé par **bcp**, un fichier de format décrit toutes les colonnes de table dans l’ordre. Vous pouvez modifier ce fichier pour réorganiser ou omettre des lignes de la table. Vous pouvez ainsi personnaliser un fichier de format en fonction d'un fichier de données dont les champs ne sont pas mappés directement aux colonnes de table. Pour plus d'informations, voir les rubriques suivantes :  
  
-   [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [Utiliser un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)   
 [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)   
 [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  
  
