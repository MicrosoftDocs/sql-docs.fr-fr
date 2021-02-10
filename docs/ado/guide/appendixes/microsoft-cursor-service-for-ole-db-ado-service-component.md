---
description: Service de curseur Microsoft pour OLE DB (composant du service ADO)
title: Service de curseur Microsoft pour OLE DB (composant du service ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], cursor service for OLE DB
- cursor service for OLE DB [ADO]
ms.assetid: 420d0989-7cfb-4c66-a7b5-f4199d13165d
author: rothja
ms.author: jroth
ms.openlocfilehash: 12224a64ffa8d67b212deacd3bd8794d6519cfb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029383"
---
# <a name="microsoft-cursor-service-for-ole-db-overview"></a>Vue d’ensemble du service de curseur Microsoft pour OLE DB
Le service de curseur Microsoft pour OLE DB complète les fonctions de prise en charge de curseur des fournisseurs de données. Par conséquent, l’utilisateur perçoit des fonctionnalités relativement uniformes de tous les fournisseurs de données.

 Le service de curseur rend les propriétés dynamiques disponibles et améliore le comportement de certaines méthodes. Par exemple, la propriété dynamique [optimize](../../reference/ado-api/optimize-property-dynamic-ado.md) permet la création d’index temporaires pour faciliter certaines opérations, telles que la méthode [Find](../../reference/ado-api/find-method-ado.md) .

 Le service de curseur permet la prise en charge de la mise à jour par lot dans tous les cas. Il simule également des types de curseurs plus efficaces, tels que des curseurs dynamiques, lorsqu’un fournisseur de données ne peut fournir que des curseurs moins performants, tels que des curseurs statiques.

## <a name="keyword"></a>Mot clé
 Pour appeler ce composant de service, affectez à la propriété [CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md) de l’objet [Recordset](../../reference/ado-api/recordset-object-ado.md) ou [Connection](../../reference/ado-api/connection-object-ado.md) la valeur **adUseClient**.

```vb
connection.CursorLocation=adUseClient
recordset.CursorLocation=adUseClient
```

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Lorsque le service de curseur pour OLE DB est appelé, les propriétés dynamiques suivantes sont ajoutées à la collection de [Propriétés](../../reference/ado-api/properties-collection-ado.md) de l’objet **Recordset** . La liste complète des propriétés dynamiques de la **connexion** et de l’objet **Recordset** est répertoriée dans l' [index de propriétés dynamiques ADO](../../reference/ado-api/ado-dynamic-property-index.md). Les noms de propriétés de OLE DB associés, le cas échéant, sont inclus entre parenthèses après le nom de la propriété ADO.

 Les modifications apportées à certaines propriétés dynamiques ne sont pas visibles par la source de données sous-jacente après l’appel du service de curseur. Par exemple, la définition de la propriété de *délai d’attente* de la commande sur un **jeu d’enregistrements** n’est pas visible pour le fournisseur de données sous-jacent.

```vb

Recordset1.CursorLocation = adUseClient     'invokes cursor service
Recordset1.Open "authors", _
    "Provider=SQLOLEDB;Data Source=DBServer;User Id=MyUserID;" & _
    "Password=MyPassword;Initial Catalog=pubs;",,adCmdTable
Recordset1.Properties.Item("Command Time out") = 50
' 'Command Time out' property on DBServer is still default (30).

```

 Si votre application requiert le service de curseur, mais que vous devez définir des propriétés dynamiques sur le fournisseur sous-jacent, définissez les propriétés avant d’appeler le service de curseur. Les paramètres de propriété de l’objet de commande sont toujours passés au fournisseur de données sous-jacent, indépendamment de l’emplacement du curseur. Par conséquent, vous pouvez également utiliser un objet de commande pour définir les propriétés à tout moment.

> [!NOTE]
>  Le DBPROP_SERVERDATAONINSERT de propriété dynamique n’est pas pris en charge par le service de curseur, même s’il est pris en charge par le fournisseur de données sous-jacent.

|Nom de la propriété|Description|
|-------------------|-----------------|
|Recalcul automatique (DBPROP_ADC_AUTORECALC)|Pour les jeux d’enregistrements créés avec le service de mise en forme des données, cette valeur indique la fréquence à laquelle les colonnes calculées et agrégées sont calculées. La valeur par défaut (valeur = 1) doit être recalculée chaque fois que le service de mise en forme des données détermine que les valeurs ont changé. Si la valeur est 0, les colonnes calculées ou agrégées sont calculées uniquement lors de la création initiale de la hiérarchie.|
|Taille du lot (DBPROP_ADC_BATCHSIZE)|Indique le nombre d’instructions UPDATE qui peuvent être traitées par lot avant d’être envoyées au magasin de données. Plus il y a d’instructions dans un lot, moins il y a d’allers-retours vers le magasin de données.|
|Mettre en cache les lignes enfants (DBPROP_ADC_CACHECHILDROWS)|Pour les jeux d’enregistrements créés avec le service de mise en forme des données, cette valeur indique si les recordsets enfants sont stockés dans un cache pour une utilisation ultérieure.|
|Version du moteur de curseur (DBPROP_ADC_CEVER)|Indique la version du service de curseur utilisé.|
|Conserver l’état de modification (DBPROP_ADC_MAINTAINCHANGESTATUS)|Indique le texte de la commande utilisée pour resynchroniser une ou plusieurs lignes dans une jointure de tables multiples.|
|[Optimize](../../reference/ado-api/optimize-property-dynamic-ado.md)|Indique si un index doit être créé. Quand la valeur est **true**, autorise la création temporaire d’index pour améliorer l’exécution de certaines opérations.|
|[Reformer le nom](../../reference/ado-api/reshape-name-property-dynamic-ado.md)|Indique le nom de l’ensemble d' **enregistrements**. Peut être référencé dans les commandes de mise en forme des données actuelles ou suivantes.|
|[Commande Resync](../../reference/ado-api/resync-command-property-dynamic-ado.md)|Indique une chaîne de commande personnalisée qui est utilisée par la méthode [Resync](../../reference/ado-api/resync-method.md) lorsque la propriété de [table unique](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) est appliquée.|
|[Catalogue unique](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indique le nom de la base de données contenant la table référencée dans la propriété de **table unique** .|
|[Schéma unique](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indique le nom du propriétaire de la table référencée dans la propriété de **table unique** .|
|[table unique](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indique le nom d’une table dans un **jeu d’enregistrements** créé à partir de plusieurs tables qui peuvent être modifiées par des insertions, des mises à jour ou des suppressions.|
|Critères de mise à jour (DBPROP_ADC_UPDATECRITERIA)|Indique les champs de la clause **Where** utilisés pour gérer les collisions survenant pendant une mise à jour.|
|[Mise à jour de la resynchronisation](../../reference/ado-api/update-resync-property-dynamic-ado.md) (DBPROP_ADC_UPDATERESYNC)|Indique si la méthode **Resync** est implicitement appelée après la méthode [UpdateBatch](../../reference/ado-api/updatebatch-method.md) (et son comportement), lorsque la propriété de **table unique** est appliquée.|

 Vous pouvez également définir ou récupérer une propriété dynamique en spécifiant son nom en tant qu’index de la collection **Properties** . Par exemple, obtenir et imprimer la valeur actuelle de la propriété dynamique [optimize](../../reference/ado-api/optimize-property-dynamic-ado.md) , puis définir une nouvelle valeur, comme suit :

```vb
Debug.Print rs.Properties("Optimize")
rs.Properties("Optimize") = True
```

## <a name="built-in-property-behavior"></a>Comportement de la propriété intégrée
 Le service de curseur pour OLE DB affecte également le comportement de certaines propriétés intégrées.

|Nom de la propriété|Description|
|-------------------|-----------------|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|Complète les types de curseurs disponibles pour un **Recordset**.|
|[Verrou](../../reference/ado-api/locktype-property-ado.md)|Complète les types de verrous disponibles pour un **Recordset**. Active les mises à jour par lots.|
|[Sort](../../reference/ado-api/sort-property.md)|Spécifie un ou plusieurs noms de champs sur lesquels le **Recordset** est trié, et indique si chaque champ est trié par ordre croissant ou décroissant.|

## <a name="method-behavior"></a>Comportement de la méthode
 Le service de curseur pour OLE DB active ou affecte le comportement de la méthode [Append](../../reference/ado-api/append-method-ado.md) de l’objet [Field](../../reference/ado-api/field-object.md) . et les méthodes [Open](../../reference/ado-api/open-method-ado-recordset.md), [Resync](../../reference/ado-api/resync-method.md), [UpdateBatch](../../reference/ado-api/updatebatch-method.md)et [Save](../../reference/ado-api/save-method.md) de l’objet **Recordset** .