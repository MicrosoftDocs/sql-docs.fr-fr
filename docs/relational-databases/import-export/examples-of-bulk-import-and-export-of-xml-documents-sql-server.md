---
title: Importer et exporter en bloc des documents XML
description: Exemples d’importation et d’exportation en bloc de documents XML avec SQL Server
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- field terminators [SQL Server]
- bulk importing [SQL Server], data formats
- row terminators [SQL Server]
- OPENROWSET function, XML bulk load
- terminators [SQL Server]
- bulk exporting [SQL Server], data formats
- XML bulk load [SQL Server]
ms.assetid: dff99404-a002-48ee-910e-f37f013d946d
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 5e2a52bcfa2e95aa364bbdf3079e2f8666f0e1e0
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978786"
---
# <a name="examples-of-bulk-import-and-export-of-xml-documents-sql-server"></a>Exemples d’importation et d’exportation en bloc de documents XML (SQL Server)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

## <a name="top"></a>

Vous pouvez importer en bloc des documents XML vers une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou les exporter en bloc à partir d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette rubrique fournit des exemples de chaque.

Pour importer des données en bloc à partir d'un fichier de données dans une table ou une vue non partitionnée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez utiliser les méthodes suivantes :  
  
- utilitaire **bcp**  
   Vous pouvez aussi faire appel à l’utilitaire **bcp** pour exporter des données à partir de n’importe quel emplacement d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenant une instruction SELECT, vues partitionnées comprises.  

- BULK INSERT  
  
- INSERT ... SELECT * FROM OPENROWSET(BULK...)  

Pour plus d'informations, consultez les rubriques ci-dessous.
- [Importer et exporter des données en bloc à l'aide de l'utilitaire bcp (SQL Server).](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)
- [Importer des données en bloc à l’aide de BULK INSERT ou OPENROWSET(BULK...) (SQL Server).](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md) 
- [Comment importer XML dans SQL Server avec le composant de chargement en masse XML.](https://mskb.pkisolutions.com/kb/316005)
- [Collections de schémas XML (SQL Server)](../xml/xml-schema-collections-sql-server.md)
  
## <a name="examples"></a>Exemples  
 Voici les exemples :  
  
-  [A. Importation en bloc de données XML sous forme de flux d’octets binaires](#binary_byte_stream)  
  
-  [B. Importation en bloc de données XML dans une ligne existante](#existing_row)  
  
-  [C. Importation en bloc de données XML à partir d’un fichier contenant une DTD](#file_contains_dtd)  
  
- [D. Spécification explicite de la marque de fin de champ à l’aide d’un fichier de format](#field_terminator_in_format_file)  
  
-  [E. Exportation en bloc de données XML](#bulk_export_xml_data)  
  
## <a name="bulk-importing-xml-data-as-a-binary-byte-stream"></a><a name="binary_byte_stream"></a>Importation en bloc de données XML sous forme de flux d'octets binaires  
 Si vous importez en bloc des données XML à partir d’un fichier contenant la déclaration d’encodage à appliquer, spécifiez l’option SINGLE_BLOB dans la clause OPENROWSET(BULK...). Cette option permet à l’analyseur XML de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’importer les données conformément au schéma d’encodage spécifié dans la déclaration XML.  
  
#### <a name="sample-table"></a>Exemple de table  
 Pour tester l’exemple A ci-dessous, créez l’exemple de table `T`.  
  
```sql
USE tempdb  
CREATE TABLE T (IntCol int, XmlCol xml);  
GO  
```  
  
#### <a name="sample-data-file"></a>Fichier de données d'exemple  
 Avant de pouvoir exécuter l’exemple A, vous devez créer un fichier encodé en UTF-8 (`C:\SampleFolder\SampleData3.txt`) : ce dernier contient l’instance d’exemple suivante qui spécifie le schéma d’encodage `UTF-8` .  
  
```xml  
<?xml version="1.0" encoding="UTF-8"?>  
<Root>  
          <ProductDescription ProductModelID="5">  
             <Summary>Some Text</Summary>  
          </ProductDescription>  
</Root>  
```  
  
#### <a name="example-a"></a>Exemple A  
 Cet exemple utilise l'option `SINGLE_BLOB` dans une instruction `INSERT ... SELECT * FROM OPENROWSET(BULK...)` pour importer des données à partir d'un fichier nommé `SampleData3.txt` et insérer une instance XML dans la table de colonne unique, la table d'exemple `T`.  
  
```sql
INSERT INTO T(XmlCol)  
SELECT * FROM OPENROWSET(  
   BULK 'c:\SampleFolder\SampleData3.txt',  
   SINGLE_BLOB) AS x;  
```  
  
#### <a name="remarks"></a>Notes  
 En utilisant SINGLE_BLOB dans ce cas, vous évitez toute discordance entre l'encodage du document XML (tel qu'il est spécifié par la déclaration d'encodage XML) et la page de codes de type chaîne inhérente au serveur.  
  
 Si vous utilisez les types de données NCLOB ou CLOB et que vous rencontrez un conflit de pages de codes ou d'encodage, effectuez l'une des opérations suivantes :  
  
-   supprimer la déclaration XML pour réussir l'importation du contenu du fichier de données XML ;  
  
-   spécifier, dans l'option CODEPAGE de la requête, une page de codes qui correspond au schéma d'encodage utilisé dans la déclaration XML ;  
  
-   faire coïncider, ou corriger, les paramètres de classement avec un schéma d'encodage XML non-Unicode.  
  
 [&#91;Haut&#93;](#top)  
  
##  <a name="bulk-importing-xml-data-in-an-existing-row"></a><a name="existing_row"></a> Importation en bloc de données XML dans une ligne existante  
 Cet exemple utilise le fournisseur d'ensemble de lignes en bloc `OPENROWSET` pour ajouter une instance XML à une ligne ou des lignes existantes dans la table d'exemple `T`.  
  
> [!NOTE]  
>  Pour exécuter cet exemple, vous devez d'abord terminer le script de test de l'exemple A. Cet exemple crée la table `tempdb.dbo.T` et importe des données en bloc à partir de `SampleData3.txt`.  
  
#### <a name="sample-data-file"></a>Fichier de données d'exemple  
 L'exemple B utilise une version modifiée du fichier de données d'exemple `SampleData3.txt` à partir de l'exemple précédent. Pour exécuter cet exemple, modifiez le contenu de ce fichier comme suit :  
  
```xml
<Root>  
          <ProductDescription ProductModelID="10">  
             <Summary>Some New Text</Summary>  
          </ProductDescription>  
</Root>  
```  
  
#### <a name="example-b"></a>Exemple B  
  
```sql  
-- Query before update shows initial state of XmlCol values.  
SELECT * FROM T  
UPDATE T  
SET XmlCol =(  
SELECT * FROM OPENROWSET(  
   BULK 'C:\SampleFolder\SampleData3.txt',  
           SINGLE_BLOB  
) AS x  
)  
WHERE IntCol = 1;  
GO  
```  
  
 [&#91;Haut&#93;](#top)  
  
## <a name="bulk-importing-xml-data-from-a-file-that-contains-a-dtd"></a><a name="file_contains_dtd"></a> Importation en bloc de données XML à partir d'un fichier contenant une DTD  
  
> [!IMPORTANT]  
>  Il est recommandé de ne pas activer la prise en charge des définitions de type de document (DTD) si celle-ci n'est pas nécessaire à votre environnement XML. En effet, son activation augmente la zone de surface attaquable de votre serveur qui peut se retrouver exposé à une attaque de déni de service. Si vous devez activer la prise en charge DTD, vous pouvez réduire ce risque lié à la sécurité en traitant uniquement des documents XML approuvés.  
  
 Si vous essayez d'utiliser une commande [bcp](../../tools/bcp-utility.md) pour importer des données XML à partir d'un fichier qui contient une DTD, une erreur semblable à celle-ci peut se produire :  
  
 "SQLState = 42000, NativeError = 6359"  
  
 "Erreur = [Microsoft][SQL Server Native Client][SQL Server]L'analyse de XML avec des DTD de sous-ensemble internes n'est pas autorisée. Utilisez CONVERT avec l'option de style 2 pour permettre une prise en charge limitée des DTD de sous-ensemble internes."  
  
 "Échec de la copie BCP %s"  
  
 Pour résoudre ce problème, vous pouvez importer des données XML à partir d'un fichier de données qui contient une DTD à l'aide de la fonction `OPENROWSET(BULK...)` et en spécifiant l'option `CONVERT` dans la clause `SELECT` de la commande. La syntaxe de base pour la commande est la suivante :  
  
 `INSERT ... SELECT CONVERT(...) FROM OPENROWSET(BULK...)`  
  
#### <a name="sample-data-file"></a>Fichier de données d'exemple  
 Avant de pouvoir tester cet exemple d’importation en bloc, créez un fichier (`C:\temp\Dtdfile.xml`) qui contient l’instance d’exemple suivant :  
  
```xml 
<!DOCTYPE DOC [<!ATTLIST elem1 attr1 CDATA "defVal1">]><elem1>January</elem1>  
```  
  
#### <a name="sample-table"></a>Exemple de table  
 L'exemple C utilise la table d'exemple `T1` créée par l'instruction `CREATE TABLE` suivante :  
  
```sql  
USE tempdb;  
CREATE TABLE T1(XmlCol xml);  
GO  
```  
  
#### <a name="example-c"></a>Exemple C  
 Cet exemple utilise `OPENROWSET(BULK...)` et spécifie l'option `CONVERT` dans la clause `SELECT` pour importer les données XML à partir du fichier `Dtdfile.xml` dans la table d'exemple `T1`.  
  
```sql
INSERT T1  
  SELECT CONVERT(xml, BulkColumn, 2) FROM   
    OPENROWSET(Bulk 'c:\temp\Dtdfile.xml', SINGLE_BLOB) [rowsetresults];  
```  
  
 Une fois l'instruction `INSERT` exécutée, la DTD est supprimée du code XML, puis stockée dans la table `T1` .  
  
 [&#91;Haut&#93;](#top)  
  
## <a name="specifying-the-field-terminator-explicitly-using-a-format-file"></a><a name="field_terminator_in_format_file"></a> Spécification explicite de la marque de fin de champ à l'aide d'un fichier de format  
 L'exemple suivant montre l'importation en bloc du document XML suivant, `Xmltable.dat`.  
  
#### <a name="sample-data-file"></a>Fichier de données d'exemple  
 Le document dans le fichier `Xmltable.dat` contient deux valeurs XML, une pour chaque ligne. La première valeur XML est encodée en UTF-16, et la seconde en UTF-8.  
  
 Le contenu de ce fichier de données est présenté dans le vidage hexadécimal suivant :  
  
```  
FF FE 3C 00 3F 00 78 00-6D 00 6C 00 20 00 76 00  *..\<.?.x.m.l. .v.*  
65 00 72 00 73 00 69 00-6F 00 6E 00 3D 00 22 00  *e.r.s.i.o.n.=.".*  
31 00 2E 00 30 00 22 00-20 00 65 00 6E 00 63 00  *1...0.". .e.n.c.*  
6F 00 64 00 69 00 6E 00-67 00 3D 00 22 00 75 00  *o.d.i.n.g.=.".u.*  
74 00 66 00 2D 00 31 00-36 00 22 00 3F 00 3E 00  *t.f.-.1.6.".?.>.*  
3C 00 72 00 6F 00 6F 00-74 00 3E 00 A2 4F 9C 76  *\<.r.o.o.t.>..O.v*  
0C FA 77 E4 80 00 89 00-00 06 90 06 91 2E 9B 2E  *..w.............*  
99 34 A2 34 86 00 83 02-92 20 7F 02 4E C5 E4 A3  *.4.4..... ..N...*  
34 B2 B7 B3 B7 FE F8 FF-F8 00 3C 00 2F 00 72 00  *4.........\<./.r.*  
6F 00 6F 00 74 00 3E 00-00 00 00 00 7A EF BB BF  *o.o.t.>.....z...*  
3C 3F 78 6D 6C 20 76 65-72 73 69 6F 6E 3D 22 31  *\<?xml version="1*  
2E 30 22 20 65 6E 63 6F-64 69 6E 67 3D 22 75 74  *.0" encoding="ut*  
66 2D 38 22 3F 3E 3C 72-6F 6F 74 3E E4 BE A2 E7  *f-8"?><root>....*  
9A 9C EF A8 8C EE 91 B7-C2 80 C2 89 D8 80 DA 90  *................*  
E2 BA 91 E2 BA 9B E3 92-99 E3 92 A2 C2 86 CA 83  *................*  
E2 82 92 C9 BF EC 95 8E-EA 8F A4 EB 88 B4 EB 8E  *................*  
B7 EF BA B7 EF BF B8 C3-B8 3C 2F 72 6F 6F 74 3E  *.........</root>*  
00 00 00 00 7A                                   *....z*  
```  
  
#### <a name="sample-table"></a>Exemple de table  
 Durant une importation ou une exportation en bloc d’un document XML, vous devez utiliser une [marque de fin de champ](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md) qu’il est impossible de trouver dans les documents ; par exemple, une série de quatre valeurs Null (`\0`) suivie de la lettre `z`: `\0\0\0\0z`.  
  
 Cet exemple illustre l'utilisation de cette marque de fin de champ pour la table d'exemple `xTable` . Pour créer cette table d'exemple, utilisez les instructions `CREATE TABLE` suivantes :  
  
```sql
USE tempdb;  
CREATE TABLE xTable (xCol xml);  
GO  
```  
  
#### <a name="sample-format-file"></a>Fichier de format d'exemple  
 Le délimiteur de fin de champ doit être spécifié dans le fichier de format. L'exemple D utilise un fichier de format non XML intitulé `Xmltable.fmt` qui contient le code suivant :  
  
```  
9.0  
1  
1       SQLBINARY     0       0       "\0\0\0\0z"    1     xCol         ""  
```  
  
 Vous pouvez utiliser ce fichier de format pour importer en bloc des documents XML dans la table `xTable` à l'aide d'une commande `bcp` ou d'une instruction `BULK INSERT` ou `INSERT ... SELECT * FROM OPENROWSET(BULK...)` .  
  
#### <a name="example-d"></a>Exemple D  
 Cet exemple utilise le fichier de format `Xmltable.fmt` dans une instruction `BULK INSERT` pour importer le contenu d'un fichier de données XML intitulé `Xmltable.dat`.  
  
```sql
BULK INSERT xTable   
FROM 'C:\Xmltable.dat'  
WITH (FORMATFILE = 'C:\Xmltable.fmt');  
GO  
```  
  
 [&#91;Haut&#93;](#top)  
  
## <a name="bulk-exporting-xml-data"></a><a name="bulk_export_xml_data"></a> Exportation en bloc de données XML  
 L’exemple suivant utilise [bcp](../../tools/bcp-utility.md) pour exporter en bloc des données XML à partir de la table créée dans l’exemple précédent, à l’aide du même fichier de format XML. Dans la commande `bcp` suivante, `<server_name>` et `<instance_name>` représentent des espaces réservés qui doivent être remplacés par les valeurs appropriées :  
  
```cmd
bcp bulktest..xTable out a-wn.out -N -T -S<server_name>\<instance_name>  
```  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'enregistre pas l'encodage XML lorsque les données XML sont conservées dans la base de données. Par conséquent, l'encodage original des champs XML n'est pas disponible lorsque les données XML sont exportées. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise l’encodage UTF-16 durant l’exportation de données XML.  
  

## <a name="see-also"></a>Voir aussi  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Clause SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-clause-transact-sql.md)   
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)  
  
  
