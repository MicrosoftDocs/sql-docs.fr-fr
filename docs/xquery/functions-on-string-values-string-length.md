---
title: Fonction string-length (XQuery) | Microsoft Docs
description: Découvrez comment utiliser la fonction XQuery String-length ().
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- string-length function
- fn:string-length function
ms.assetid: 7cd69c8b-cf2c-478c-b9a3-e0e14e1aa8aa
author: rothja
ms.author: jroth
ms.openlocfilehash: 588e3a4dc19d502da07d23971aea7b987e49a8ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344991"
---
# <a name="functions-on-string-values---string-length"></a>Fonctions sur les valeurs de chaîne : string-length
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne la longueur de la chaîne en caractères.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:string-length() as xs:integer  
fn:string-length($arg as xs:string?) as xs:integer  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Chaîne source dont la longueur doit être calculée.  
  
## <a name="remarks"></a>Notes  
 Si la valeur de *$arg* est une séquence vide, une valeur **XS : Integer** égale à 0 est retournée.  
  
 Le comportement de paires de substitution dans les fonctions XQuery dépend du niveau de compatibilité de la base de données. Si le niveau de compatibilité est 110 ou supérieur, chaque paire de substitution est comptée comme un caractère unique. Pour les premiers niveaux de compatibilité, ils sont comptés comme deux caractères. Pour plus d’informations, consultez [niveau de compatibilité ALTER database &#40;&#41;Transact-SQL ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) et [prise en charge d’Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).  
  
 Si la valeur contient un caractère Unicode composé de 4 octets et représenté par deux caractères de substitution, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] compte ces caractères de substitution séparément.  
  
 La **chaîne-length ()** sans paramètre ne peut être utilisée qu’à l’intérieur d’un prédicat. Par exemple, la requête suivante retourne l' `ROOT` élément <> :  
  
```  
DECLARE @x xml;  
SET @x='<ROOT>Hello</ROOT>';  
SELECT @x.query('/ROOT[string-length()=5]');  
```  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Le comportement de la paire de substitution dans des fonctions XQuery dépend du niveau de compatibilité de la base de données et, dans certains cas, de l'URI de l'espace de noms par défaut des fonctions. Pour plus d’informations, consultez la section « les fonctions XQuery prennent en charge les substitutions » dans la rubrique [modifications avec rupture des fonctionnalités de moteur de base de données dans SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Consultez également [niveau de compatibilité ALTER database &#40;&#41;Transact-SQL ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) et la [prise en charge d’Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-string-length-xquery-function-to-retrieve-products-with-long-summary-descriptions"></a>R. Utilisation de la fonction XQuery string-length() pour extraire les produits dont la description résumée présente une certaine longueur  
 Pour les produits dont la description résumée est supérieure à 50 caractères, la requête suivante extrait l’ID de produit, la longueur de la description résumée et le résumé lui-même, l' `Summary` élément <>.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' as pd)  
SELECT CatalogDescription.query('  
      <Prod ProductID= "{ /pd:ProductDescription[1]/@ProductModelID }" >  
       <LongSummary SummaryLength =   
           "{string-length(string( (/pd:ProductDescription/pd:Summary)[1] )) }" >  
           { string( (/pd:ProductDescription/pd:Summary)[1] ) }  
       </LongSummary>  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.value('string-length( string( (/pd:ProductDescription/pd:Summary)[1]))', 'decimal') > 200;  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La condition de la clause WHERE extrait uniquement les lignes où la description résumée stockée dans le document XML comprend plus de 200 caractères. Elle utilise la [méthode value () (type de données XML)](../t-sql/xml/value-method-xml-data-type.md).  
  
-   La clause SELECT construit simplement le document XML de votre choix. Elle utilise la [méthode Query () (type de données XML)](../t-sql/xml/query-method-xml-data-type.md) pour construire le XML et spécifier l’expression XQuery nécessaire pour récupérer les données du document XML.  
  
 Voici un extrait du résultat :  
  
```  
Result  
-------------------  
<Prod ProductID="19">  
      <LongSummary SummaryLength="214">Our top-of-the-line competition   
             mountain bike. Performance-enhancing options include the  
             innovative HL Frame, super-smooth front suspension, and   
             traction for all terrain.  
      </LongSummary>  
</Prod>  
...  
```  
  
### <a name="b-using-the-string-length-xquery-function-to-retrieve-products-whose-warranty-descriptions-are-short"></a>B. Utilisation de la fonction XQuery string-length() pour extraire les produits dont la description de la garantie est courte  
 Pour les produits dont la description de la garantie contient moins de 20 caractères, la requête suivante extrait du code XML qui comprend l’ID de produit, la longueur, la description de la garantie et l' `Warranty` élément <> lui-même.  
  
 La garantie est l'une des caractéristiques du produit. Un `Warranty` élément enfant <> facultatif suit le <`Features`> élément.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for   $ProdDesc in /pd:ProductDescription,  
            $pf in $ProdDesc/pd:Features/wm:Warranty  
      where string-length( string(($pf/wm:Description)[1]) ) < 20  
      return   
          <Prod >  
             { $ProdDesc/@ProductModelID }  
             <ShortFeature FeatureDescLength =   
                             "{string-length( string(($pf/wm:Description)[1]) ) }" >  
                 { $pf }  
             </ShortFeature>  
          </Prod>  
     ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription')=1;  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   **PD** et **WM** sont les préfixes d’espace de noms utilisés dans cette requête. Ils identifient les mêmes espaces de noms que ceux utilisés dans le document interrogé.  
  
-   La requête XQuery spécifie une boucle FOR imbriquée. La boucle FOR externe est requise, car vous souhaitez récupérer les attributs **ProductModelID** de l’élément <`ProductDescription`>. La boucle FOR interne est requise car vous ne souhaitez obtenir que les produits dont la description de la garantie comprend moins de 20 caractères.  
  
 Voici le résultat partiel :  
  
```  
Result  
-------------------------  
<Prod ProductModelID="19">  
  <ShortFeature FeatureDescLength="15">  
    <wm:Warranty   
       xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
      <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
      <wm:Description>parts and labor</wm:Description>  
    </wm:Warranty>  
   </ShortFeature>  
</Prod>  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
