---
description: OpenSchema, méthode
title: OpenSchema, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::OpenSchema
- Connection15::raw_OpenSchema
helpviewer_keywords:
- OpenSchema method [ADO]
ms.assetid: 850cf3ce-f18f-4e7c-8597-96c1dc504866
author: rothja
ms.author: jroth
ms.openlocfilehash: 9b5ca92e42a68639f6b9e3044894ec779109fff5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170678"
---
# <a name="openschema-method"></a>OpenSchema, méthode
Obtient des informations de schéma de base de données à partir du fournisseur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset = connection.OpenSchema(QueryType, Criteria, SchemaID)  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne un objet [Recordset](./recordset-object-ado.md) qui contient des informations de schéma. Le **jeu d’enregistrements** s’ouvre en tant que curseur statique en lecture seule. Le *TypeRequête* détermine les colonnes qui s’affichent dans le **jeu d’enregistrements**.  
  
#### <a name="parameters"></a>Paramètres  
 *QueryType*  
 Toute valeur [SchemaEnum](./schemaenum.md) qui représente le type de requête de schéma à exécuter.  
  
 *Critères*  
 Facultatif. Tableau de contraintes de requête pour chaque option *QueryType* , comme indiqué dans [SchemaEnum](./schemaenum.md).  
  
 *SchemaID*  
 GUID d’une requête de schéma de fournisseur non définie par la spécification OLE DB. Ce paramètre est obligatoire si *QueryType* est défini sur **adSchemaProviderSpecific**; dans le cas contraire, il n’est pas utilisé.  
  
## <a name="remarks"></a>Notes  
 La méthode **OpenSchema** retourne des informations auto-descriptives sur la source de données, telles que les tables qui se trouvent dans la source de données, les colonnes des tables et les types de données pris en charge.  
  
 L’argument *QueryType* est un GUID qui indique que les colonnes (schémas) sont retournées. La spécification de OLE DB a une liste complète de schémas.  
  
 L’argument *Criteria* limite les résultats d’une requête de schéma. *Critères* spécifie un tableau de valeurs qui doivent se produire dans un sous-ensemble correspondant de colonnes, appelées colonnes de contrainte, dans le **jeu d’enregistrements** résultant.  
  
 La constante **adSchemaProviderSpecific** est utilisée pour l’argument *QueryType* si le fournisseur définit ses propres requêtes de schéma non standard en dehors de celles listées précédemment. Lorsque cette constante est utilisée, l’argument *SchemaID* est requis pour transmettre le GUID de la requête de schéma à exécuter. Si *QueryType* est défini sur **AdSchemaProviderSpecific** mais que *SchemaID* n’est pas fourni, une erreur se produit.  
  
 Les fournisseurs ne sont pas tenus de prendre en charge toutes les requêtes de schéma standard OLE DB. Plus précisément, seuls **adSchemaTables**, **adSchemaColumns** et **adSchemaProviderTypes** sont requis par la spécification OLE DB. Toutefois, le fournisseur n’est pas tenu de prendre en charge les contraintes de *critères* répertoriées précédemment pour ces requêtes de schéma.  
  
> [!NOTE]
>  **Utilisation des services de données distants** La méthode **OpenSchema** n’est pas disponible sur un objet de [connexion](./connection-object-ado.md) côté client.  
  
> [!NOTE]
>  Dans Visual Basic, les colonnes qui ont un entier non signé de 4 octets (DBTYPE UI4) dans le **jeu d’enregistrements** retourné par la méthode **OpenSchema** sur l’objet de **connexion** ne peuvent pas être comparées à d’autres variables. Pour plus d’informations sur les types de données OLE DB, consultez [types de données dans OLE DB (OLE DB)](/previous-versions/windows/desktop/ms714931(v=vs.85)) et [annexe A : types de données](/previous-versions/windows/desktop/ms723969(v=vs.85)) dans le Guide de référence du programmeur Microsoft OLE DB.  
  
> [!NOTE]
>  **Utilisateurs Visual C/C++** Lorsque vous n’utilisez pas de curseurs côté client, la récupération du « ORDINAL_POSITION » d’un schéma de colonne dans ADO retourne une variante de type VT_R8 dans MDAC 2,7, MDAC 2,8 et Windows DAC (Windows Data Access Components) 6,0, alors que le type utilisé dans MDAC 2,6 était VT_I4. Les programmes écrits pour MDAC 2,6 qui recherchent uniquement un Variant retourné de type VT_I4 obtenir un zéro pour chaque ordinal s’il est exécuté sous MDAC 2,7, MDAC 2,8 et Windows DAC 6,0 sans modification. Cette modification a été apportée parce que le type de données retourné par OLE DB est DBTYPE_UI4, et dans le type de VT_I4 signé, il n’y a pas assez de place pour contenir toutes les valeurs possibles sans risque de troncation, ce qui entraîne une perte de données.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [OpenSchema, exemple de méthode (VB)](./openschema-method-example-vb.md)   
 [OpenSchema, exemple de méthode (VC + +)](./openschema-method-example-vc.md)   
 [Open, méthode (connexion ADO)](./open-method-ado-connection.md)   
 [Open, méthode (ADO record)](./open-method-ado-record.md)   
 [Open, méthode (objet Recordset ADO)](./open-method-ado-recordset.md)   
 [Open, méthode (objet Stream ADO)](./open-method-ado-stream.md)   
 [Annexe A : Fournisseurs](../../guide/appendixes/appendix-a-providers.md)