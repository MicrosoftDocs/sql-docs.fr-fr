---
title: Charger des données XML | Microsoft Docs
description: Découvrez plusieurs méthodes de transfert de données XML dans des bases de données SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XML data [SQL Server], loading
- loading XML data
ms.assetid: d1741e8d-f44e-49ec-9f14-10208b5468a7
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 19a0518477c37b9f45f22cdf6b5ded1464cf145e
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250668"
---
# <a name="load-xml-data"></a>Charger des données XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Vous pouvez transférer des données XML dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de plusieurs manières. Par exemple :  
  
-   Si vos données figurent dans une colonne de type [n]text ou image, dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez importer la table par le biais de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Modifiez ensuite le type de colonne en XML à l'aide de l'instruction ALTER TABLE.  
  
-   Vous pouvez copier vos données en bloc à partir d’une autre base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de bcp out, puis les insérer en bloc dans la base de données de version ultérieure à l’aide de bcp in.  
  
-   Si vous avez des données dans des colonnes relationnelles d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , créez une table avec une colonne [n]text et, éventuellement, une colonne de clé primaire pour un identificateur de ligne. Utilisez la programmation côté client pour récupérer les données XML générées sur le serveur par la clause FOR XML et placez-les dans la colonne **[n]text** . Ensuite, utilisez les techniques citées précédemment pour transférer les données vers une base de données de version ultérieure. Vous pouvez choisir d’écrire directement le code XML dans une colonne XML de la base de données de version ultérieure.  
  
## <a name="bulk-loading-xml-data"></a>Chargement en masse de données XML  
 Vous pouvez charger en masse des données XML sur le serveur en utilisant les fonctions de chargement en masse de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme bcp. OPENROWSET vous permet de charger des données dans une colonne XML à partir de fichiers. L'exemple suivant illustre ce comportement :  
  
##### <a name="example-loading-xml-from-files"></a>Exemple : Chargement de données XML à partir de fichiers  
 Cet exemple montre comment insérer une ligne dans la table T. La valeur de la colonne XML est chargée à partir du fichier C:\MyFile\xmlfile.xml en tant que CLOB, et la colonne integer prend la valeur 10.  
  
```  
INSERT INTO T  
SELECT 10, xCol  
FROM    (SELECT *      
    FROM OPENROWSET (BULK 'C:\MyFile\xmlfile.xml', SINGLE_CLOB)   
 AS xCol) AS R(xCol)  
```  
  
## <a name="text-encoding"></a>Encodage de texte  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stocke les données XML au format Unicode (UTF-16). Les données XML extraites du serveur se présentent au format UTF-16. Si vous souhaitez un encodage différent, vous devez convertir les données extraites au format voulu. Il arrive que les données XML soient encodées différemment. Si tel est le cas, vous devez prêter une attention particulière au chargement des données. Par exemple :  
  
-   Si votre texte XML est en Unicode (UCS-2, UTF-16), vous pouvez l'affecter à une colonne, une variable ou un paramètre XML sans aucun problème.  
  
-   Si l'encodage ne se fait pas en Unicode et qu'il est implicite, du fait d'une page de codes source, la page de codes de la chaîne dans la base de données doit être identique (ou du moins compatible) aux points de code que vous souhaitez charger. Si nécessaire, utilisez COLLATE. Si aucune page de codes serveur de la sorte n'existe, vous devez ajouter une déclaration XML explicite mentionnant l'encodage correct.  
  
-   Pour utiliser un encodage explicite, utilisez le type **varbinary()** , qui n’a aucune interaction avec les pages de codes, ou utilisez un type chaîne de la page de codes appropriée. Ensuite, assignez les données à une colonne, une variable ou un paramètre XML.  
  
### <a name="example-explicitly-specifying-an-encoding"></a>Exemple : Spécification implicite d’un encodage  
 Supposez que vous avez un document XML, vcdoc, stocké au format **varchar(max)** , qui ne comporte aucune déclaration XML explicite. L’instruction ci-dessous permet d’ajouter une déclaration XML mentionnant l’encodage « iso8859-1 », de concaténer le document XML, de convertir le résultat au format **varbinary(max)** de façon à conserver la représentation en octets, puis enfin de le convertir au format XML. Ainsi, le processeur XML peut analyser les données conformément à l'encodage spécifié « iso8859-1 » et générer la représentation UTF-16 correspondante pour les valeurs de chaîne.  
  
```  
SELECT CAST(   
CAST (('<?xml version="1.0" encoding="iso8859-1"?>'+ vcdoc) AS VARBINARY (MAX))   
 AS XML)  
```  
  
### <a name="string-encoding-incompatibilities"></a>Incompatibilités d'encodage de chaîne  
 Si vous copiez et collez du code XML sous forme de littéral chaîne dans la fenêtre d'éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous risquez de rencontrer des incompatibilités d'encodage de la chaîne [N]VARCHAR. Tout dépendra de l'encodage de votre instance XML. Dans bien des cas, vous serez amené à supprimer la déclaration XML. Par exemple :  
  
```  
<?xml version="1.0" encoding="UTF-8"?>  
  <xsd:schema ...  
```  
  
 Vous devez ensuite inclure un N pour transformer l'instance XML en une instance Unicode. Par exemple :  
  
```  
-- Assign XML instance to a variable.  
DECLARE @X XML  
SET @X = N'...'  
-- Insert XML instance into an xml type column.  
INSERT INTO T VALUES (N'...')  
-- Create an XML schema collection  
CREATE XML SCHEMA COLLECTION XMLCOLL1 AS N'<xsd:schema ... '  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
