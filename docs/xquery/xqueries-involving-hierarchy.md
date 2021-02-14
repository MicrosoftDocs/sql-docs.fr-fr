---
title: Requêtes XQuery impliquant une hiérarchie | Microsoft Docs
description: Affichez des exemples de requêtes XQuery qui impliquent des hiérarchies.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- hierarchies [XQuery]
- XQuery, hierarchies
ms.assetid: 6953d8b7-bad8-4b64-bf7b-12fa4f10f65c
author: rothja
ms.author: jroth
ms.openlocfilehash: fdebb93459b9519eb6495054908208de610e8222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352793"
---
# <a name="xqueries-involving-hierarchy"></a>Requêtes XQuery impliquant une hiérarchie
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  La plupart des colonnes de type **XML** dans la base de données **AdventureWorks** sont des documents semi-structurés. Par conséquent, les documents stockés dans chaque ligne peuvent avoir un aspect différent. Les exemples de requêtes fournis dans cette rubrique montrent comment extraire des informations de ces divers documents.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-from-the-manufacturing-instructions-documents-retrieve-work-center-locations-together-with-the-first-manufacturing-step-at-those-locations"></a>R. Extraction, à partir des instructions de fabrication, des postes de travail ainsi que de la première étape de fabrication réalisée sur ces différents postes  
 Pour le modèle de produit 7, la requête construit le code XML qui comprend l' `ManuInstr` élément <>, avec les attributs **ProductModelID** et **ProductModelName** , et un ou plusieurs <`Location`> éléments enfants.  
  
 Chaque <`Location` élément> a son propre ensemble d’attributs et un <`step`> élément enfant. Ce <`step`> élément enfant est la première étape de fabrication sur l’emplacement du poste de travail.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   \<ManuInstr  ProdModelID = "{sql:column("Production.ProductModel.ProductModelID") }"   
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
            {   
              for $wc in //AWMI:root/AWMI:Location  
              return  
                <Location>  
                 {$wc/@* }  
                 <step1> { string( ($wc//AWMI:step)[1] ) } </step1>  
                </Location>  
            }  
          </ManuInstr>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le mot clé **namespace** dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) définit un préfixe d’espace de noms. Ce préfixe est utilisé ultérieurement dans le corps de la requête.  
  
-   Les jetons de basculement de contexte, {) et (}, sont utilisés pour faire passer la requête de la construction XML à sa propre évaluation.  
  
-   **SQL : Column ()** est utilisé pour inclure une valeur relationnelle dans le code XML en cours de construction.  
  
-   Lors de la construction de l' `Location` élément <>, $WC/@ * récupère tous les attributs de l’emplacement du centre de travail.  
  
-   La fonction **String ()** retourne la valeur de chaîne de l' `step` élément <>.  
  
 Voici un extrait du résultat :  
  
```xml
<ManuInstr ProdModelID="7" ProductModelName="HL Touring Frame">  
   <Location LocationID="10" SetupHours="0.5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
     <step1>Insert aluminum sheet MS-2341 into the T-85A   
             framing tool.</step1>  
   </Location>  
   <Location LocationID="20" SetupHours="0.15"   
            MachineHours="2" LaborHours="1.75" LotSize="1">  
      <step1>Assemble all frame components following   
             blueprint 1299.</step1>  
   </Location>  
...  
</ManuInstr>   
```  
  
### <a name="b-find-all-telephone-numbers-in-the-additionalcontactinfo-column"></a>B. Recherche de tous les numéros de téléphone de la colonne AdditionalContactInfo  
 La requête suivante récupère des numéros de téléphone supplémentaires pour un contact client spécifique en recherchant l’élément <> dans la hiérarchie entière `telephoneNumber` . Étant donné que l' `telephoneNumber` élément <> peut apparaître n’importe où dans la hiérarchie, la requête utilise le descendant et l’opérateur Self (//) dans la recherche.  
  
```sql
SELECT AdditionalContactInfo.query('  
 declare namespace ci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
 declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
for $ph in /ci:AdditionalContactInfo//act:telephoneNumber  
   return  
      $ph/act:number  
') as x  
FROM  Person.Contact  
WHERE ContactID = 1  
```  
  
 Voici le résultat obtenu :  
  
```xml
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  112-111-1111  
\</act:number>  
```  
  
 Pour récupérer uniquement les numéros de téléphone de niveau supérieur, en particulier les <`telephoneNumber`> éléments enfants de <`AdditionalContactInfo`>, l’expression for de la requête devient  
  
 `for $ph in /ci:AdditionalContactInfo/act:telephoneNumber`.  
  
## <a name="see-also"></a>Voir aussi  
 [Notions de base de XQuery](../xquery/xquery-basics.md)   
 [Construction XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
  
