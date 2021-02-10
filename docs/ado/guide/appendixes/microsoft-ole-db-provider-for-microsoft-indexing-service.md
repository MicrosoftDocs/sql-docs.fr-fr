---
description: Vue d’ensemble du fournisseur Microsoft OLE DB pour le service d’indexation Microsoft
title: Fournisseur Microsoft OLE DB pour le service d’indexation Microsoft | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Indexing Service provider [ADO]
- providers [ADO], OLE DB provider for Microsoft Indexing service
- OLE DB provider for Microsoft Indexing service [ADO]
ms.assetid: f86a0598-5097-471b-8318-d2c859d085f2
author: rothja
ms.author: jroth
ms.openlocfilehash: f7d2204a2d91e862616713dbfee6eec3d6d3e8ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029303"
---
# <a name="microsoft-ole-db-provider-for-microsoft-indexing-service-overview"></a>Vue d’ensemble du fournisseur Microsoft OLE DB pour le service d’indexation Microsoft
Le fournisseur Microsoft OLE DB pour le service d’indexation Microsoft fournit un accès par programmation en lecture seule au système de fichiers et aux données Web indexées par le service d’indexation Microsoft. Les applications ADO peuvent émettre des requêtes SQL pour récupérer du contenu et des informations sur les propriétés de fichier.

 Le fournisseur est libre de thread et UNICODE est activé.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, affectez à l’argument **Provider =** la valeur de la propriété [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) :

```vb
MSIDXS
```

 La lecture de la propriété [Provider](../../reference/ado-api/provider-property-ado.md) retourne également cette chaîne.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion classique pour ce fournisseur est la suivante :

```vb
"Provider=MSIDXS;Data Source=myCatalog;Locale Identifier=nnnn;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé|Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur de OLE DB pour le service d’indexation Microsoft. En général, il s’agit du seul mot clé spécifié dans la chaîne de connexion.|
|**Source de données**|Spécifie le nom du catalogue du service d’indexation. Si ce mot clé n’est pas spécifié, le catalogue système par défaut est utilisé.|
|**Identificateur de paramètres régionaux**|Spécifie un nombre 32 bits unique (par exemple, 1033) qui spécifie les préférences relatives à la langue de l’utilisateur. Si ce mot clé n’est pas spécifié, l’identificateur de paramètres régionaux par défaut est utilisé.|

## <a name="command-text"></a>Texte de la commande
 La syntaxe de requête SQL du service d’indexation se compose d’extensions de l’instruction SQL-92 **Select** et de ses clauses **from** et **Where** . Les résultats de la requête sont retournés via OLE DB ensembles de lignes, qui peuvent être consommés par ADO et manipulés en tant qu’objets [Recordset](../../reference/ado-api/recordset-object-ado.md) .

 Vous pouvez rechercher des mots ou expressions exacts, ou utiliser des caractères génériques pour rechercher des modèles ou des tiges de mots. La logique de recherche peut être basée sur des décisions booléennes, des termes pondérés ou une proximité à d’autres mots. Vous pouvez également effectuer une recherche en « texte libre », qui recherche les correspondances en fonction de la signification, plutôt que des mots exacts.

 Le dialecte de la commande spécifique est entièrement documenté dans la documentation des langages de requête pour le service d’indexation.

 Le fournisseur n’accepte pas les appels de procédure stockée ou les noms de tables simples (par exemple, la propriété [CommandType](../../reference/ado-api/commandtype-property-ado.md) sera toujours **adCmdText**).

## <a name="recordset-behavior"></a>Comportement du Recordset
 Les tableaux suivants répertorient les fonctionnalités disponibles avec un objet **Recordset** ouvert avec ce fournisseur. Seul le type de curseur statique (**adOpenStatic**) est disponible.

 Pour plus d’informations sur le comportement du **Recordset** pour la configuration de votre fournisseur, exécutez la méthode [supports](../../reference/ado-api/supports-method.md) et Énumérez la collection [Properties](../../reference/ado-api/properties-collection-ado.md) du **Recordset** pour déterminer si des propriétés dynamiques spécifiques au fournisseur sont présentes.

 **Disponibilité des propriétés standard du Recordset ADO :**

|Propriété|Disponibilité|
|--------------|------------------|
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|lecture/écriture|
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|lecture/écriture|
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|en lecture seule|
|[Business](../../reference/ado-api/bof-eof-properties-ado.md)|en lecture seule|
|[Signet](../../reference/ado-api/bookmark-property-ado.md)*|lecture/écriture|
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|lecture/écriture|
|[Ait](../../reference/ado-api/cursorlocation-property-ado.md)|toujours **adUseServer**|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|toujours **adOpenStatic**|
|[EditMode](../../reference/ado-api/editmode-property.md)|toujours **adEditNone**|
|[EOF](../../reference/ado-api/bof-eof-properties-ado.md)|en lecture seule|
|[Filter](../../reference/ado-api/filter-property.md)|lecture/écriture|
|[Verrou](../../reference/ado-api/locktype-property-ado.md)|lecture/écriture|
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|non disponible|
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|lecture/écriture|
|[PageCount](../../reference/ado-api/pagecount-property-ado.md)|en lecture seule|
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|lecture/écriture|
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|en lecture seule|
|[Source](../../reference/ado-api/source-property-ado-recordset.md)|lecture/écriture|
|[State](../../reference/ado-api/state-property-ado.md)|en lecture seule|
|[État](../../reference/ado-api/status-property-ado-recordset.md)|en lecture seule|

 \*Les signets doivent être activés sur le fournisseur afin que cette fonctionnalité existe sur le **Recordset**.

 **Disponibilité des méthodes de l’ensemble d’enregistrements ADO standard :**

|Méthode|Disponible ?|
|------------|----------------|
|[AddNew](../../reference/ado-api/addnew-method-ado.md)|Non|
|[Annuler](../../reference/ado-api/cancel-method-ado.md)|Oui|
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|Non|
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|Non|
|[Répliqué](../../reference/ado-api/clone-method-ado.md)|Oui|
|[Close](../../reference/ado-api/close-method-ado.md)|Oui|
|[Supprimer](../../reference/ado-api/delete-method-ado-recordset.md)|Non|
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|Oui|
|[Déplacer](../../reference/ado-api/move-method-ado.md)|Oui|
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Oui|
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)|Oui|
|[Ouvrir](../../reference/ado-api/open-method-ado-recordset.md)|Oui|
|[Requery](../../reference/ado-api/requery-method.md)|Oui|
|[Resynchroniser](../../reference/ado-api/resync-method.md)|Oui|
|[Prise en charge](../../reference/ado-api/supports-method.md)|Oui|
|[Mettre à jour](../../reference/ado-api/update-method.md)|Non|
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|Non|

 Pour obtenir des détails d’implémentation spécifiques et des informations fonctionnelles sur le fournisseur Microsoft OLE DB pour le service d’indexation Microsoft, consultez le [Guide du programmeur OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85))ou visitez la page services Web du site Web Windows NT Server.

## <a name="see-also"></a>Voir aussi
 [CommandType, propriété (ADO)](../../reference/ado-api/commandtype-property-ado.md) [ConnectionString, propriété (ADO](../../reference/ado-api/connectionstring-property-ado.md) [) Properties collection (ADO)](../../reference/ado-api/properties-collection-ado.md) [Provider, propriété](../../reference/ado-api/provider-property-ado.md) (ADO) [Recordset, objet (ADO)](../../reference/ado-api/recordset-object-ado.md) [prend en charge la méthode](../../reference/ado-api/supports-method.md)